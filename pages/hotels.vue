<template>
  <div class="home-container">
    <nav class="navbar navbar-expand-lg navbar-light bg-white border-bottom shadow-sm">
      <div class="container">
        <a class="navbar-brand d-flex align-items-center" href="/">
          <img
            src="https://i.postimg.cc/VLDp5PGV/Screenshot-2026-07-29-005610.png"
            alt="Roomio"
            width="40"
            height="40"
            class="me-2"
          />
          <span class="roomio-logo">Roomio</span>
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav ms-auto">
            <li class="nav-item"><a class="nav-link text-muted" href="/">หน้าหลัก</a></li>
            <li class="nav-item"><a class="nav-link text-coral fw-medium" href="/hotels">ห้องพัก</a></li>
            <li class="nav-item"><a class="nav-link text-muted" href="/bookings">จองห้องพัก</a></li>
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle text-muted" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">อื่นๆ</a>
              <ul class="dropdown-menu">
                <li><a class="dropdown-item" href="#">ติดต่อเรา</a></li>
                <li><a class="dropdown-item" href="#">เกี่ยวกับเรา</a></li>
              </ul>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <!-- รายการโรงแรม -->
    <div class="container py-4" v-if="!selectedHotel">
      <div v-if="searchQuery" class="d-flex align-items-center justify-content-between mb-4 bg-light p-3 rounded-3">
        <div>
          <span class="text-muted me-2">ผลการค้นหาสำหรับ:</span>
          <strong class="text-coral fs-5">"{{ searchQuery }}"</strong>
        </div>
        <button class="btn btn-sm btn-outline-secondary" @click="clearSearch">ล้างการค้นหา</button>
      </div>

      <div class="row g-4" v-if="filteredHotels.length > 0">
        <div class="col-md-4" v-for="hotel in filteredHotels" :key="hotel.id">
          <div class="card h-100 shadow-sm cursor-pointer" @click="selectHotel(hotel)">
            <img :src="hotel.image" class="card-img-top" style="height: 220px; object-fit: cover" />
            <div class="card-body">
              <p class="text-warning mb-1">⭐ {{ hotel.rating }} ดีมาก</p>
              <h6 class="fw-bold">{{ hotel.name }}</h6>
              <p class="fw-semibold text-coral mt-2 mb-0">THB {{ (hotel.price || 0).toLocaleString() }}</p>
            </div>
          </div>
        </div>
      </div>

      <div v-else class="text-center py-5">
        <i class="bi bi-search fs-1 text-muted"></i>
        <h5 class="mt-3 text-muted">ไม่พบที่พักที่ตรงกับคำค้นหา "{{ searchQuery }}"</h5>
        <button class="btn btn-coral mt-2" @click="clearSearch">ดูที่พักทั้งหมด</button>
      </div>
    </div>

    <!-- รายละเอียดโรงแรมและห้องพัก -->
    <div class="container py-5" v-else>
      <button class="btn btn-outline-secondary mb-4" @click="selectedHotel = null">← กลับไปหน้ารายการ</button>

      <div class="hotel-gallery">
        <div class="main-image">
          <img 
            :src="selectedHotel.images[0]" 
            class="img-fluid rounded" 
            alt="Main hotel image" 
            @click="openImageModal(selectedHotel.images[0])" 
          />
        </div>
        <div class="sub-images">
          <div 
            v-for="(img, i) in selectedHotel.images.slice(1, 5)" 
            :key="i" 
            class="sub-image"
            :class="{ 'last-image': i === 3 }"
            @click="openImageModal(img)"
          >
            <img :src="img" class="img-fluid rounded" :alt="`Hotel image ${i + 2}`" />
            <div v-if="i === 3 && selectedHotel.images.length > 5" class="image-overlay">
              <span class="overlay-text">+{{ selectedHotel.images.length - 5 }} รูปอื่นๆ</span>
            </div>
          </div>
        </div>
      </div>

      <div class="d-flex justify-content-between align-items-start mt-4">
        <div>
          <h4 class="fw-bold mb-1">{{ selectedHotel.name }}</h4>
          <p class="text-warning fw-medium mb-1">⭐ {{ selectedHotel.rating }} / 10 ดีมาก</p>
        </div>
        <div class="text-end">
          <p class="hotel-price-detail mb-0">ราคาเริ่มต้นที่<br />
            <span class="fs-4 fw-bold text-coral">THB {{ (selectedHotel.price || 0).toLocaleString() }}</span>
          </p>
        </div>
      </div>

      <div class="row mt-4 align-items-start">
        <div class="col-md-6 facilities-container">
          <h6 class="fw-bold mb-3">สิ่งอำนวยความสะดวก</h6>
          <div class="row g-3">
            <div class="col-12 col-sm-6 col-lg-4" v-for="facility in facilities" :key="facility.text">
              <div class="detail-icon">
                <i :class="facility.icon"></i>
                <span>{{ facility.text }}</span>
              </div>
            </div>
          </div>
        </div>

        <div class="col-md-6 nearby-container">
          <h6 class="fw-bold mb-3">สถานที่ใกล้เคียง</h6>
          <ul>
            <li v-for="place in nearbyPlaces" :key="place">
              <i class="bi bi-geo-alt-fill text-coral me-2"></i>{{ place }}
            </li>
          </ul>
        </div>
      </div>

      <div class="row mt-4">
        <div class="col-12 mb-4" v-for="(room, index) in rooms" :key="index">
          <div class="room-card-new" :class="{ 'selected-room': selectedRoom && selectedRoom.name === room.name }">
            <div class="row g-3 align-items-stretch">
              <div class="col-lg-4">
                <div class="room-image-frame">
                  <img :src="room.image" :alt="room.name + ' Room'" class="img-fluid rounded" />
                </div>
              </div>
              <div class="col-lg-8">
                <div class="room-details-frame">
                  <div class="row h-100">
                    <div class="col-md-8 d-flex flex-column justify-content-center">
                      <h5 class="fw-bold text-coral mb-3">{{ room.name }}</h5>
                      <div class="d-flex align-items-center gap-2 mb-2" v-if="room.beds">
                        <i class="bi bi-bed fs-5 text-coral"></i>
                        <span class="text-dark">{{ room.beds }}</span>
                      </div>
                      <div class="form-check mb-2">
                        <input 
                          class="form-check-input" 
                          type="checkbox" 
                          :id="'extraBed-' + index"
                          v-model="room.extraBedSelected"
                          @change="updateRoomPrice(index)"
                        />
                        <label class="form-check-label" :for="'extraBed-' + index">
                          {{ room.extraBed }}
                        </label>
                      </div>
                      <div v-if="room.singleBed" class="d-flex align-items-center gap-2 mb-2">
                        <i class="bi bi-bed fs-6 text-muted"></i>
                        <span class="text-muted">{{ room.singleBed }}</span>
                      </div>
                      <div class="d-flex align-items-center gap-2">
                        <i class="bi bi-people fs-5 text-coral"></i>
                        <span>{{ room.guests }}</span>
                      </div>
                    </div>
                    <div class="col-md-4 text-end d-flex flex-column justify-content-center align-items-end">
                      <div class="mb-3">
                        <span class="hotel-price-detail">THB <span class="fs-4 fw-bold text-coral">{{ room.price.toLocaleString() }}</span></span>
                      </div>
                      <button 
                        class="btn px-4 py-2 rounded-pill" 
                        :class="selectedRoom && selectedRoom.name === room.name ? 'btn-success' : 'btn-coral'"
                        @click="selectRoom(room)"
                      >
                        {{ selectedRoom && selectedRoom.name === room.name ? 'เลือกแล้ว' : 'เลือก' }}
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="row mt-5">
        <div class="col-12">
          <h4 class="fw-bold mb-4 text-coral">ทัวร์ท่องเที่ยวแนะนำ</h4>
          
          <div class="row mb-4">
            <div class="col-md-6 mb-3">
              <div class="card border-0 shadow-sm">
                <div class="position-relative">
                  <img src="https://images.unsplash.com/photo-1544551763-46a013bb70d5?ixlib=rb-4.0.3&w=800" 
                       class="card-img-top" style="height: 200px; object-fit: cover;" />
                  <div class="position-absolute top-0 end-0 m-2">
                    <input 
                      class="form-check-input" 
                      type="checkbox" 
                      style="width: 20px; height: 20px;" 
                      v-model="selectedTours"
                      value="phi-phi"
                      id="phi-phi"
                    />
                  </div>
                </div>
                <div class="card-body">
                  <h6 class="fw-bold mb-2">ทัวร์เกาะไผ่, เกาะจีน และเกาะปีปอ</h6>
                  <p class="text-muted small mb-2">THB 2,680</p>
                  <p class="text-muted small mb-0">เพิ่มเติม ∨</p>
                </div>
              </div>
            </div>
            
            <div class="col-md-6 mb-3">
              <div class="card border-0 shadow-sm">
                <div class="position-relative">
                  <img src="https://images.unsplash.com/photo-1564349683136-77e08dba1ef7?ixlib=rb-4.0.3&w=800" 
                       class="card-img-top" style="height: 200px; object-fit: cover;" />
                  <div class="position-absolute top-0 end-0 m-2">
                    <input 
                      class="form-check-input" 
                      type="checkbox" 
                      style="width: 20px; height: 20px;" 
                      v-model="selectedTours"
                      value="zoo"
                      id="zoo"
                    />
                  </div>
                </div>
                <div class="card-body">
                  <h6 class="fw-bold mb-2">ทัวร์สวนสัตว์ปีปอเขาเขียว</h6>
                  <p class="text-muted small mb-2">THB 1,250</p>
                  <p class="text-muted small mb-0">เพิ่มเติม ∨</p>
                </div>
              </div>
            </div>
          </div>

          <div class="tours-section">
            <h5 class="fw-bold mb-3">ทัวร์ท่องเที่ยว</h5>
            
            <div class="tour-item tour-item-bordered d-flex align-items-center mb-3 p-3">
              <div class="form-check me-3">
                <input class="form-check-input" type="checkbox" v-model="selectedTours" value="cruise" id="cruise">
              </div>
              <div class="flex-grow-1">
                <h6 class="mb-1">ทัวร์สำราญท์ทะเลแบบสำราญ</h6>
                <p class="text-muted small mb-0">เพิ่มเติม ∨</p>
              </div>
              <div class="text-end">
                <span class="fw-bold">THB 1,670</span>
              </div>
            </div>

            <div class="tour-item tour-item-bordered d-flex align-items-center mb-3 p-3">
              <div class="form-check me-3">
                <input class="form-check-input" type="checkbox" v-model="selectedTours" value="temple" id="temple">
              </div>
              <div class="flex-grow-1">
                <h6 class="mb-1">ทัวร์เกาะชาม สัตบุน</h6>
                <p class="text-muted small mb-0">เพิ่มเติม ∨</p>
              </div>
              <div class="text-end">
                <span class="fw-bold">THB 750</span>
              </div>
            </div>

            <div class="tour-item tour-item-bordered d-flex align-items-center mb-3 p-3">
              <div class="form-check me-3">
                <input class="form-check-input" type="checkbox" v-model="selectedTours" value="adventure" id="adventure">
              </div>
              <div class="flex-grow-1">
                <h6 class="mb-1">ทัวร์อิสสราย-เมืองเก่า</h6>
                <p class="text-muted small mb-0">เพิ่มเติม ∨</p>
              </div>
              <div class="text-end">
                <span class="fw-bold">THB 990</span>
              </div>
            </div>

            <div class="tour-item tour-item-bordered d-flex align-items-center mb-3 p-3">
              <div class="form-check me-3">
                <input class="form-check-input" type="checkbox" v-model="selectedTours" value="spa" id="spa">
              </div>
              <div class="flex-grow-1">
                <h6 class="mb-1">ทัวร์เรือยอร์ชต์ชมพระอาทิตย์ตก</h6>
                <p class="text-muted small mb-0">เพิ่มเติม ∨</p>
              </div>
              <div class="text-end">
                <span class="fw-bold">THB 2,500</span>
              </div>
            </div>

            <div class="d-flex justify-content-between align-items-center mt-4 p-3 bg-light rounded shadow-sm">
              <div>
                <span class="fw-bold fs-5">ราคารวม</span><br>
                <span class="fs-4 fw-bold text-coral">THB {{ totalPrice.toLocaleString() }}</span>
              </div>
              <button class="btn btn-coral px-4 py-2 rounded-pill fw-bold" @click="proceedToBooking">จองเลย</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div v-if="isModalVisible" class="image-modal-overlay" @click="closeImageModal">
    <span class="close-modal-btn">&times;</span>
    <img :src="modalImageSrc" class="modal-image" @click.stop />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()
const config = useRuntimeConfig()

const isModalVisible = ref(false)
const modalImageSrc = ref('')

const searchQuery = computed(() => {
  return (route.query.q || route.query.search || route.query.keyword || route.query.location || '').toString()
})

// คลังรูปภาพสำรองคุณภาพสูง (สำรองเผื่อ DB ไม่มีรูป)
const fallbackImagePool = [
  [
    'https://pix8.agoda.net/hotelImages/7937563/0/3cda0fb72e97b50ec09a04c8ba403764.jpeg?ce=2&s=1024x',
    'https://pix8.agoda.net/hotelImages/7937563/-1/76cb6db5284245cd9bef1fdba1b5a7e5.jpg?ca=9&ce=1&s=1024x',
    'https://q-xx.bstatic.com/xdata/images/hotel/max1024x768/197329980.jpg?k=77f414aeda7011631dbdcce26549722db9aee35d51cc9bd79342b806b30b423e&o=&s=1024x',
    'https://q-xx.bstatic.com/xdata/images/hotel/max1024x768/395810551.jpg?k=fc9c3df404198d41bbf276be686f1714b4d379e94fdca3e0d7990afd885a2426&o=&s=1024x',
    'https://images.unsplash.com/photo-1571896349842-33c89424de2d?ixlib=rb-4.0.3&w=800'
  ],
  [
    'https://pix8.agoda.net/hotelImages/90441/-1/978a2c7bd460b76eea875f3ef8131776.jpg?ca=12&ce=1&s=1024x',
    'https://cf.bstatic.com/xdata/images/hotel/270x200/217019267.jpg',
    'https://cf.bstatic.com/xdata/images/hotel/270x200/217019268.jpg',
    'https://cf.bstatic.com/xdata/images/hotel/270x200/217019269.jpg',
    'https://images.unsplash.com/photo-1540541338287-41700207dee6?ixlib=rb-4.0.3&w=800'
  ],
  [
    'https://pix8.agoda.net/hotelImages/85086/-1/4609658eba0368326b9e05acb5536c76.jpg?ca=9&ce=1&s=1024x',
    'https://cf.bstatic.com/xdata/images/hotel/270x200/469057070.jpg',
    'https://cf.bstatic.com/xdata/images/hotel/270x200/469057071.jpg',
    'https://cf.bstatic.com/xdata/images/hotel/270x200/469057072.jpg',
    'https://cf.bstatic.com/xdata/images/hotel/270x200/469057073.jpg'
  ]
]

// Mock Data สำรองกรณีไม่ได้รัน Backend
const defaultHotels = [
  {
    id: 1,
    name: 'อาณา อานันท์ รีสอร์ท แอนด์ วิลล่า',
    location: 'พัทยา นาจอมเทียน ชลบุรี',
    rating: '8.8',
    price: 2500,
    image: fallbackImagePool[0][0],
    images: fallbackImagePool[0]
  },
  {
    id: 2,
    name: 'ซีบรีซ จอมเทียน รีสอร์ท',
    location: 'พัทยา หาดจอมเทียน ชลบุรี',
    rating: '8.8',
    price: 1011,
    image: fallbackImagePool[1][0],
    images: fallbackImagePool[1]
  },
  {
    id: 3,
    name: 'ควอเตอร์ 09 บีช',
    location: 'พัทยา ชลบุรี',
    rating: '8.6',
    price: 623,
    image: fallbackImagePool[2][0],
    images: fallbackImagePool[2]
  }
]

// โหลด API เฉพาะฝั่ง browser แบบไม่บล็อกการเปลี่ยนหน้า
// Render backend อาจ cold start หรือใช้งานไม่ได้ชั่วคราว จึงกำหนด timeout
// และให้ computed ด้านล่างแสดง defaultHotels ได้ทันทีระหว่างรอ/เมื่อ API ล้มเหลว
const { data: hotelsApiData } = useFetch(`${config.public.apiBase}/hotels`, {
  server: false,
  lazy: true,
  timeout: 10000,
  default: () => []
})

const hotels = computed(() => {
  if (hotelsApiData.value && Array.isArray(hotelsApiData.value) && hotelsApiData.value.length > 0) {
    return hotelsApiData.value.map((item, index) => {
      const assignedFallback = fallbackImagePool[index % fallbackImagePool.length]

      let validImages = []
      
      // ดึงรูปจริงจาก DB โดยเช็คจากฟิลด์ image_url ตรงๆ
      if (item.hotel_images && Array.isArray(item.hotel_images)) {
        validImages = item.hotel_images
          .map(img => {
            if (typeof img === 'string') return img
            // เช็คชื่อฟิลด์ image_url ที่ตรงกับ MySQL
            return img?.image_url || img?.url || img?.path || null
          })
          // กรองเอาเฉพาะ URL ที่ขึ้นต้นด้วย http และไม่ใช่ลิงก์หลอก example.com
          .filter(url => url && typeof url === 'string' && url.trim().startsWith('http') && !url.includes('example.com'))
      }

      // ถ้าโรงแรมไหนใน DB ไม่มีรูป (เช่น ID 4, 11) หรือรูปไม่ครบ 5 ให้เติมรูปสำรองเข้าไปให้ครบ 5 รูป
      if (validImages.length < 5) {
        const extraNeeded = assignedFallback.slice(validImages.length)
        validImages = [...validImages, ...extraNeeded]
      }

      let minPrice = 1200
      if (item.rooms && Array.isArray(item.rooms)) {
        const prices = item.rooms.map(r => r?.price || r?.price_per_night).filter(p => p > 0)
        if (prices.length) minPrice = Math.min(...prices)
      }

      return {
        id: item.hotel_id || item.id,
        name: item.hotel_name || item.name,
        location: item.country || 'พัทยา ชลบุรี',
        rating: item.rating ? item.rating.toString() : '8.8',
        price: minPrice,
        image: validImages[0], // รูปหลักหน้าการ์ด
        images: validImages,   // รูปทั้งหมด 5 รูปหน้ารายละเอียด
        raw: item
      }
    })
  }
  return defaultHotels
})

const filteredHotels = computed(() => {
  const query = searchQuery.value.trim().toLowerCase()
  if (!query) return hotels.value

  return hotels.value.filter(hotel => {
    const matchName = hotel.name ? hotel.name.toLowerCase().includes(query) : false
    const matchLocation = hotel.location ? hotel.location.toLowerCase().includes(query) : false
    return matchName || matchLocation
  })
})

const clearSearch = () => {
  router.push('/hotels')
}

const facilities = [
  { icon: 'bi bi-wifi', text: 'Wi-Fi' },
  { icon: 'bi bi-car-front', text: 'ที่จอดรถ' },
  { icon: 'bi bi-water', text: 'สระว่ายน้ำ' },
  { icon: 'bi bi-snow', text: 'เครื่องปรับอากาศ' },
  { icon: 'bi bi-telephone', text: 'รูมเซอร์วิส' },
  { icon: 'bi bi-egg-fried', text: 'อาหารเช้า' }
]

const nearbyPlaces = [
  'แหล่งช็อปปิ้ง มีโชว์พัทยา',
  'โอเชี่ยนมารีนา ยอชต์คลับ',
  'วัดนาจอมเทียน',
  'ตลาดน้ำ 4 ภาค พัทยา'
]

const rooms = ref([
  {
    name: 'Deluxe Twin',
    beds: '1 เตียงคู่',
    extraBed: 'เตียงเสริม 1 คน + 500 THB',
    extraBedSelected: false,
    guests: '2 คน',
    price: 2690,
    basePrice: 2690,
    image: 'https://pix8.agoda.net/hotelImages/7937563/126928420/fa06a7c33b69d877e39416fa7c9c2567.jpg?ce=0&s=1024x'
  },
  {
    name: 'Deluxe Family',
    beds: '1 เตียงคู่',
    extraBed: 'เตียงเสริม 1 คน + 500 THB',
    extraBedSelected: false,
    singleBed: '1 เตียงเดี่ยว',
    guests: '3 คน',
    price: 3370,
    basePrice: 3370,
    image: 'https://q-xx.bstatic.com/xdata/images/hotel/max1024x768/225106213.jpg?k=b60b34646280ba60ed44c6188e5ebcf345f9acbe15459da7412ccd6192587ab8&o=&s=1024x'
  }
])

const updateRoomPrice = (index) => {
  const room = rooms.value[index]
  if (room.extraBedSelected) {
    room.price = room.basePrice + 500
  } else {
    room.price = room.basePrice
  }
}

const selectedHotel = ref(null)
const selectedRoom = ref(null)
const selectedTours = ref([])

const tourPrices = {
  'phi-phi': 2680,
  'zoo': 1250,
  cruise: 1670,
  temple: 750,
  adventure: 990,
  spa: 2500
}

const totalPrice = computed(() => {
  let total = 0
  if (selectedRoom.value) {
    total += selectedRoom.value.price
  }
  const tourTotal = selectedTours.value.reduce((sum, tour) => {
    return sum + (tourPrices[tour] || 0)
  }, 0)
  total += tourTotal
  return total
})

const selectHotel = (hotel) => {
  selectedHotel.value = hotel
  selectedRoom.value = null
  selectedTours.value = []
  rooms.value.forEach(room => {
    room.extraBedSelected = false
    room.price = room.basePrice
  })
}

const selectRoom = (room) => {
  selectedRoom.value = room
}

const proceedToBooking = () => {
  if (!selectedRoom.value) {
    alert('กรุณาเลือกห้องพักก่อนทำการจอง')
    return
  }
  
  const bookingData = {
    hotel: selectedHotel.value,
    room: selectedRoom.value,
    tours: selectedTours.value,
    tourPrices: tourPrices,
    totalPrice: totalPrice.value
  }
  
  router.push({
    path: '/bookings',
    query: { data: JSON.stringify(bookingData) }
  })
}

const openImageModal = (imageUrl) => {
  modalImageSrc.value = imageUrl
  isModalVisible.value = true
}

const closeImageModal = () => {
  isModalVisible.value = false
  modalImageSrc.value = ''
}
</script>
