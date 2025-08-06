<template>
  <div class="my-bookings-container">
    <nav class="navbar navbar-expand-lg navbar-light bg-white border-bottom">
      <div class="container">
        <NuxtLink class="navbar-brand d-flex align-items-center" to="/">
          <img
            src="https://scontent.fbkk35-1.fna.fbcdn.net/v/t39.30808-6/495154397_1657828194852162_1327971961573207698_n.jpg?stp=dst-jpg_tt6&cstp=mx1024x1024&ctp=p526x296&_nc_cat=108&cb=99be929b-878c9f95&ccb=1-7&_nc_sid=833d8c&_nc_ohc=jkxD-Q-HDL0Q7kNvwHBLEkr&_nc_oc=AdlZO7NMcvihuzRCCR1EgJCfEGgULaIGYL3FX5T1p3krThyRYcaBBNeMLOQ4vrEnZdTq_Oe3D0ZOUGlCnTtGIE5s&_nc_zt=23&_nc_ht=scontent.fbkk35-1.fna&_nc_gid=-qmoDtwbKZDzBIdcSCiUgw&oh=00_AfRikzESLuh455Qw92WeEYfZ91G3b-McYiwlNaFgIYDqog&oe=688AE719"
            alt="Roomio"
            width="40"
            height="40"
            class="me-2"
          />
          <span class="roomio-logo">Roomio</span>
        </NuxtLink>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav ms-auto">
            <li class="nav-item"><NuxtLink class="nav-link text-muted" to="/">หน้าหลัก</NuxtLink></li>
            <li class="nav-item"><NuxtLink class="nav-link text-muted" to="/hotels">ห้องพัก</NuxtLink></li>
            <li class="nav-item"><NuxtLink class="nav-link text-coral fw-medium" to="/bookings">จองห้องพัก</NuxtLink></li>
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

    <div class="container py-5">
      <div class="text-center mb-4">
        <i class="bi bi-check-circle-fill text-success" style="font-size: 4rem;"></i>
        <h2 class="mt-3 fw-bold">การจองสำเร็จ!</h2>
        <p class="text-muted">ขอบคุณที่ใช้บริการ Roomio ของเรา</p>
      </div>

      <div v-if="latestBooking" class="card shadow-sm mx-auto" style="max-width: 600px;">
        <div class="card-body p-4">
          <div class="row align-items-center mb-3">
            <div class="col-md-4">
              <img :src="latestBooking.hotel.image" class="img-fluid rounded" alt="Hotel Image" />
            </div>
            <div class="col-md-8">
              <h5 class="fw-bold mb-1">{{ latestBooking.hotel.name }}</h5>
              <p class="text-muted small mb-0">{{ latestBooking.room.name }}</p>
              <p class="text-muted small mb-0">{{ latestBooking.room.beds }}</p>
            </div>
          </div>

          <hr>

          <div class="row g-2 mb-3">
            <div class="col-12 d-flex align-items-center">
              <i class="bi bi-person-fill text-coral me-2"></i>
              <span>ชื่อผู้จอง: {{ latestBooking.customer.firstName }} {{ latestBooking.customer.lastName }}</span>
            </div>
            <div class="col-12 d-flex align-items-center">
              <i class="bi bi-telephone-fill text-coral me-2"></i>
              <span>เบอร์โทรศัพท์: {{ latestBooking.customer.phone }}</span>
            </div>
            <div class="col-12 d-flex align-items-center">
              <i class="bi bi-envelope-fill text-coral me-2"></i>
              <span>อีเมล: {{ latestBooking.customer.email }}</span>
            </div>
          </div>

          <hr>

          <div class="row g-2 mb-3">
            <div class="col-12 d-flex align-items-center">
              <i class="bi bi-calendar-check-fill text-coral me-2"></i>
              <span>เช็คอิน: {{ formatDate(latestBooking.dates.checkIn) }}</span>
            </div>
            <div class="col-12 d-flex align-items-center">
              <i class="bi bi-calendar-x-fill text-coral me-2"></i>
              <span>เช็คเอาท์: {{ formatDate(latestBooking.dates.checkOut) }}</span>
            </div>
            <div class="col-12 d-flex align-items-center">
              <i class="bi bi-people-fill text-coral me-2"></i>
              <span>จำนวนผู้เข้าพัก: {{ latestBooking.guests }} คน, {{ latestBooking.rooms }} ห้อง</span>
            </div>
            <div class="col-12 d-flex align-items-center" v-if="latestBooking.tours && latestBooking.tours.length">
              <i class="bi bi-compass-fill text-coral me-2"></i>
              <span>ทัวร์ที่เลือก: {{ latestBooking.tours.map(getTourName).join(', ') }}</span>
            </div>
          </div>

          <hr>

          <div class="d-flex justify-content-between align-items-center">
            <span class="fs-5 fw-bold">ราคารวม:</span>
            <span class="fs-4 fw-bold text-coral">{{ formatPrice(latestBooking.pricing.grandTotal) }}</span>
          </div>
        </div>
      </div>
      <div v-else class="alert alert-info text-center mx-auto" style="max-width: 600px;">
        ไม่พบข้อมูลการจองล่าสุด
      </div>

      <div class="text-center mt-4">
        <NuxtLink to="/" class="btn btn-coral py-2 px-4">กลับไปหน้าหลัก</NuxtLink>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const latestBooking = ref(null);

const tourNames = {
  'phi-phi': 'ทัวร์เกาะไผ่, เกาะจีน และเกาะปีปอ',
  'zoo': 'ทัวร์สวนสัตว์ปีปอเขาเขียว',
  'cruise': 'ทัวร์สำราญท์ทะเลแบบสำราญ',
  'temple': 'ทัวร์เกาะชาม สัตบุน',
  'adventure': 'ทัวร์อิสสราย-เมืองเก่า',
  'spa': 'ทัวร์เรือยอร์ชต์ชมพระอาทิตย์ตก'
}

const getTourName = (key) => {
  return tourNames[key] || key
}

const formatPrice = (price) => {
  return (price || 0).toLocaleString() + ' ฿'
}

const formatDate = (dateString) => {
  if (!dateString) return ''
  const date = new Date(dateString)
  return date.toLocaleDateString('th-TH', {
    year: 'numeric',
    month: 'short',
    day: 'numeric'
  })
}

onMounted(() => {
  const existingBookings = JSON.parse(localStorage.getItem('userBookings') || '[]');
  if (existingBookings.length > 0) {
    latestBooking.value = existingBookings[existingBookings.length - 1];
  }
});
</script>
