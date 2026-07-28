<template>
  <div class="home-container">
    <nav class="navbar navbar-expand-lg navbar-light bg-white border-bottom fixed-top shadow-sm">
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
            <li class="nav-item">
              <a class="nav-link text-coral fw-medium" href="/">หน้าหลัก</a>
            </li>
            <li class="nav-item">
              <a class="nav-link text-muted" href="/hotels">ห้องพัก</a>
            </li>
            <li class="nav-item">
              <a class="nav-link text-muted" href="/bookings">จองห้องพัก</a>
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
                <div class="mb-4 position-relative search-autocomplete-container">
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
                      @focus="showSuggestions = true"
                      required
                    />
                  </div>

                  <div 
                    v-if="showSuggestions && filteredSuggestions.length > 0" 
                    class="autocomplete-dropdown position-absolute w-100 bg-white border rounded-3 shadow-lg mt-1 text-start overflow-hidden"
                    style="z-index: 1050; max-height: 250px; overflow-y: auto;"
                  >
                    <div
                      v-for="(item, index) in filteredSuggestions"
                      :key="index"
                      class="suggestion-item p-3 border-bottom d-flex align-items-center cursor-pointer hover-bg-light"
                      @click="selectSuggestion(item.name)"
                      style="cursor: pointer;"
                    >
                      <i class="bi bi-geo-alt-fill text-coral me-3 fs-5"></i>
                      <div>
                        <div class="fw-bold text-dark">{{ item.name }}</div>
                        <small class="text-muted">{{ item.location }}</small>
                      </div>
                    </div>
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

const adults = ref(2)
const children = ref(0)
const rooms = ref(1)
const showDropdown = ref(false)
const searchLocation = ref('')
const checkinDate = ref('')
const checkoutDate = ref('')
const isSearching = ref(false)

const showSuggestions = ref(false)

const allLocations = ref([
  { name: 'อาณา อานันท์ รีสอร์ท แอนด์ วิลล่า', location: 'นาจอมเทียน, พัทยา' },
  { name: 'ซีบรีซ จอมเทียน รีสอร์ท', location: 'หาดจอมเทียน, พัทยา' },
  { name: 'ควอเตอร์ 09 บีช', location: 'พัทยา' },
  { name: 'พัทยา', location: 'ชลบุรี' },
  { name: 'หาดจอมเทียน', location: 'พัทยา' }
])

const filteredSuggestions = computed(() => {
  const query = searchLocation.value.trim().toLowerCase()
  if (!query) return []
  
  return allLocations.value.filter(item => 
    item.name.toLowerCase().includes(query) || 
    item.location.toLowerCase().includes(query)
  )
})

function selectSuggestion(name) {
  searchLocation.value = name
  showSuggestions.value = false
}

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
  showSuggestions.value = false

  try {
    await navigateTo({
      path: '/hotels',
      query: {
        q: searchLocation.value.trim(),
        search: searchLocation.value.trim(),
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

function handleClickOutside(event) {
  const guestDropdown = document.querySelector('.guest-dropdown')
  if (guestDropdown && !guestDropdown.contains(event.target)) {
    showDropdown.value = false
  }

  const searchContainer = document.querySelector('.search-autocomplete-container')
  if (searchContainer && !searchContainer.contains(event.target)) {
    showSuggestions.value = false
  }
}

onMounted(() => {
  const todayDate = new Date()
  const tomorrowDate = new Date(todayDate)
  tomorrowDate.setDate(tomorrowDate.getDate() + 1)
  
  checkinDate.value = todayDate.toISOString().split('T')[0]
  checkoutDate.value = tomorrowDate.toISOString().split('T')[0]
  
  document.addEventListener('click', handleClickOutside)
})

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside)
})
</script>

<style scoped>
.suggestion-item:hover {
  background-color: #f8f9fa;
}
</style>