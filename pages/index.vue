<template>
  <div class="home-container">
    <nav class="navbar navbar-expand-lg navbar-light bg-white border-bottom fixed-top shadow-sm">
      <div class="container">
        <a class="navbar-brand d-flex align-items-center" href="#">
          <img
            src="https://scontent.fbkk35-1.fna.fbcdn.net/v/t39.30808-6/495154397_1657828194852162_1327971961573207698_n.jpg?stp=dst-jpg_tt6&cstp=mx1024x1024&ctp=p526x296&_nc_cat=108&cb=99be929b-878c9f95&ccb=1-7&_nc_sid=833d8c&_nc_ohc=jkxD-Q-HDL0Q7kNvwHBLEkr&_nc_oc=AdlZO7NMcvihuzRCCR1EgJCfEGgULaIGYL3FX5T1p3krThyRYcaBBNeMLOQ4vrEnZdTq_Oe3D0ZOUGlCnTtGIE5s&_nc_zt=23&_nc_ht=scontent.fbkk35-1.fna&_nc_gid=-qmoDtwbKZDzBIdcSCiUgw&oh=00_AfRikzESLuh455Qw92WeEYfZ91G3b-McYiwlNaFgIYDqog&oe=688AE719"
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
            <li class="nav-item">
              <a class="nav-link text-coral fw-medium" href="/">หน้าหลัก</a>
            </li>
            <li class="nav-item">
              <a class="nav-link text-muted" href="hotels">ห้องพัก</a>
            </li>
            <li class="nav-item">
              <a class="nav-link text-muted" href="#">จองห้องพัก</a>
            </li>
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle text-muted" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">
                อื่นๆ
              </a>
              <ul class="dropdown-menu">
                <li><a class="dropdown-item" href="#">ติดต่อเรา</a></li>
                <li><a class="dropdown-item" href="#">เกี่ยวกับเรา</a></li>
              </ul>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <div class="hero-section">
      <div class="container">
        <div class="row justify-content-center">
          <div class="col-lg-8 text-center">
            <h1 class="hero-title mb-3">
               <span class="text-coral">ยินดีต้อนรับสู่ Roomio</span>
            </h1>
            <p class="hero-subtitle">
              เว็บไซต์จองที่พักที่สะดวก ทันสมัย และน่าเชื่อถือ
            </p>

            <div class="search-form-container bg-white rounded-4 shadow-lg p-4">
              <form @submit.prevent="handleSearch">
                <div class="mb-4">
                  <label class="form-label text-start d-block fw-medium">
                    ค้นหาที่พัก
                  </label>
                  <div class="input-group input-group-lg">
                    <span class="input-group-text bg-white border-end-0">
                      <i class="bi bi-search text-muted"></i>
                    </span>
                    <input
                      v-model="searchLocation"
                      type="text"
                      class="form-control border-start-0 search-input" 
                      placeholder="ใส่ชื่อสถานที่หรือชื่อที่พัก"
                      required
                    />
                  </div>
                </div>

                <div class="row mb-4">
                  <div class="col-md-6 mb-3 mb-md-0">
                    <label class="form-label text-start d-block fw-medium">วันที่เช็คอิน</label>
                    <input
                      v-model="checkinDate"
                      type="date"
                      class="form-control form-control-lg"
                      :min="today"
                      @change="updateCheckoutMinDate"
                      required
                    />
                  </div>
                  <div class="col-md-6">
                    <label class="form-label text-start d-block fw-medium">วันที่เช็คเอาท์</label>
                    <input
                      v-model="checkoutDate"
                      type="date"
                      class="form-control form-control-lg"
                      :min="minCheckoutDate"
                      required
                    />
                  </div>
                </div>

                <div class="mb-4">
                  <label class="form-label fw-medium text-start d-block">ผู้เข้าพักและห้องพัก</label>
                  <div class="position-relative guest-dropdown">
                    <div class="input-group input-group-lg" @click="toggleDropdown">
                      <span class="input-group-text bg-white">
                        <i class="bi bi-people text-muted"></i>
                      </span>
                      <input
                        type="text"
                        class="form-control"
                        readonly
                        :value="guestSummaryText"
                        style="cursor: pointer;"
                      />
                      <span class="input-group-text bg-white">
                        <i class="bi bi-chevron-down text-muted" :class="{ 'rotate-180': showDropdown }"></i>
                      </span>
                    </div>


                    <div v-if="showDropdown" class="dropdown-panel position-absolute w-100 mt-2 bg-white border rounded-3 shadow-lg p-3" style="z-index: 1000;">
                      <div class="d-flex justify-content-between align-items-center py-3 border-bottom">
                        <div class="d-flex align-items-center">
                          <i class="bi bi-person-fill me-3 text-coral fs-5"></i>
                          <div>
                            <div class="fw-medium">ผู้ใหญ่</div>
                            <small class="text-muted">อายุ 13 ปีขึ้นไป</small>
                          </div>
                        </div>
                        <div class="btn-group">
                          <button type="button" class="btn btn-outline-secondary btn-sm" @click="decrementAdults" :disabled="adults <= 1">−</button>
                          <button type="button" class="btn btn-light btn-sm px-3" disabled>{{ adults }}</button>
                          <button type="button" class="btn btn-outline-secondary btn-sm" @click="incrementAdults">+</button>
                        </div>
                      </div>

                      <div class="d-flex justify-content-between align-items-center py-3 border-bottom">
                        <div class="d-flex align-items-center">
                          <i class="bi bi-person me-3 text-info fs-5"></i>
                          <div>
                            <div class="fw-medium">เด็ก</div>
                            <small class="text-muted">อายุ 0-12 ปี</small>
                          </div>
                        </div>
                        <div class="btn-group">
                          <button type="button" class="btn btn-outline-secondary btn-sm" @click="decrementChildren" :disabled="children <= 0">−</button>
                          <button type="button" class="btn btn-light btn-sm px-3" disabled>{{ children }}</button>
                          <button type="button" class="btn btn-outline-secondary btn-sm" @click="incrementChildren">+</button>
                        </div>
                      </div>

                      <div class="d-flex justify-content-between align-items-center py-3">
                        <div class="d-flex align-items-center">
                          <i class="bi bi-door-closed me-3 text-warning fs-5"></i>
                          <div>
                            <div class="fw-medium">ห้อง</div>
                            <small class="text-muted">จำนวนห้องพัก</small>
                          </div>
                        </div>
                        <div class="btn-group">
                          <button type="button" class="btn btn-outline-secondary btn-sm" @click="decrementRooms" :disabled="rooms <= 1">−</button>
                          <button type="button" class="btn btn-light btn-sm px-3" disabled>{{ rooms }}</button>
                          <button type="button" class="btn btn-outline-secondary btn-sm" @click="incrementRooms">+</button>
                        </div>
                      </div>

                      <div class="text-end mt-3">
                        <button type="button" class="btn btn-coral btn-sm px-4" @click="closeDropdown">
                          เสร็จสิ้น
                        </button>
                      </div>
                    </div>
                  </div>
                </div>

                <button type="submit" class="btn btn-coral btn-lg w-100 py-3 fw-medium" :disabled="isSearching">
                  <span v-if="isSearching">
                    <i class="bi bi-hourglass-split me-2"></i>
                    กำลังค้นหา...
                  </span>
                  <span v-else>
                    <i class="bi bi-search me-2"></i>
                    ค้นหา
                  </span>
                </button>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const adults = ref(2)
const children = ref(0)
const rooms = ref(1)
const showDropdown = ref(false)
const searchLocation = ref('')
const checkinDate = ref('')
const checkoutDate = ref('')
const isSearching = ref(false)

const today = computed(() => {
  return new Date().toISOString().split('T')[0]
})

const minCheckoutDate = computed(() => {
  if (checkinDate.value) {
    const checkin = new Date(checkinDate.value)
    checkin.setDate(checkin.getDate() + 1)
    return checkin.toISOString().split('T')[0]
  }
  return today.value
})

const guestSummaryText = computed(() => {
  const totalGuests = adults.value + children.value
  return `${totalGuests} ผู้เข้าพัก, ${rooms.value} ห้อง`
})

function toggleDropdown() {
  showDropdown.value = !showDropdown.value
}

function closeDropdown() {
  showDropdown.value = false
}

function incrementAdults() {
  adults.value++
}

function decrementAdults() {
  if (adults.value > 1) {
    adults.value--
  }
}

function incrementChildren() {
  children.value++
}

function decrementChildren() {
  if (children.value > 0) {
    children.value--
  }
}

function incrementRooms() {
  rooms.value++
}

function decrementRooms() {
  if (rooms.value > 1) {
    rooms.value--
  }
}

function updateCheckoutMinDate() {
  if (checkoutDate.value && checkinDate.value) {
    const checkin = new Date(checkinDate.value)
    const checkout = new Date(checkoutDate.value)
    
    if (checkout <= checkin) {
      const nextDay = new Date(checkin)
      nextDay.setDate(nextDay.getDate() + 1)
      checkoutDate.value = nextDay.toISOString().split('T')[0]
    }
  }
}


async function handleSearch() {
  if (!searchLocation.value.trim()) {
    alert('กรุณาใส่ชื่อสถานที่หรือชื่อที่พัก')
    return
  }

  if (!checkinDate.value || !checkoutDate.value) {
    alert('กรุณาเลือกวันที่เช็คอินและเช็คเอาท์')
    return
  }

  isSearching.value = true

  try {า
    await new Promise(resolve => setTimeout(resolve, 1500))
    const searchParams = {
      location: searchLocation.value,
      checkin: checkinDate.value,
      checkout: checkoutDate.value,
      adults: adults.value,
      children: children.value,
      rooms: rooms.value,
      nights: calculateNights()
    }

    console.log('ค้นหาที่พักด้วยข้อมูล:', searchParams)

    await router.push({
      path: '/hotels',
      query: {
        q: searchLocation.value,
        checkin: checkinDate.value,
        checkout: checkoutDate.value,
        adults: adults.value.toString(),
        children: children.value.toString(),
        rooms: rooms.value.toString()
      }
    })

  } catch (error) {
    console.error('เกิดข้อผิดพลาดในการค้นหา:', error)
    alert('เกิดข้อผิดพลาดในการค้นหา กรุณาลองใหม่อีกครั้ง')
  } finally {
    isSearching.value = false
  }
}

function calculateNights() {
  if (checkinDate.value && checkoutDate.value) {
    const checkin = new Date(checkinDate.value)
    const checkout = new Date(checkoutDate.value)
    const diffTime = Math.abs(checkout - checkin)
    const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24))
    return diffDays
  }
  return 0
}

function handleClickOutside(event) {
  const dropdown = document.querySelector('.guest-dropdown')
  if (dropdown && !dropdown.contains(event.target)) {
    showDropdown.value = false
  }
}

onMounted(() => {
  const today = new Date()
  const tomorrow = new Date(today)
  tomorrow.setDate(tomorrow.getDate() + 1)
  
  checkinDate.value = today.toISOString().split('T')[0]
  checkoutDate.value = tomorrow.toISOString().split('T')[0]
  
  document.addEventListener('click', handleClickOutside)
})

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside)
})
</script>
