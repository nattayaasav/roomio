<template>
  <div class="booking-container">
    <nav class="navbar navbar-expand-lg navbar-light bg-white border-bottom">
      <div class="container">
        <NuxtLink class="navbar-brand d-flex align-items-center" to="/">
          <img
            src="https://i.postimg.cc/VLDp5PGV/Screenshot-2026-07-29-005610.png"
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

    <div v-if="loading" class="container py-5 text-center">
      <div class="spinner-border text-coral" role="status">
        <span class="visually-hidden">กำลังโหลด...</span>
      </div>
      <p class="mt-2">กำลังเตรียมข้อมูลการจอง...</p>
    </div>

    <div v-else-if="error" class="container py-5">
      <div class="alert alert-danger" role="alert">
        <h4 class="alert-heading">เกิดข้อผิดพลาด!</h4>
        <p>{{ error }}</p>
        <button class="btn btn-outline-danger" @click="goBack">กลับไปหน้าก่อนหน้า</button>
      </div>
    </div>

    <div v-else class="container py-5">
      <div class="row">
        <div class="col-12 mb-4">
          <button class="btn btn-outline-secondary" @click="goBack">
            <i class="fas fa-arrow-left"></i> กลับ
          </button>
        </div>

        <div class="col-lg-8">
          <div class="card shadow-sm">
            <div class="card-header bg-white">
              <h4 class="fw-bold mb-0">รายละเอียดการจอง</h4>
            </div>
            <div class="card-body">
              <div class="mb-4">
                <h5 class="fw-bold mb-3">ข้อมูลผู้จอง</h5>
                <div class="row g-3">
                  <div class="col-md-6">
                    <label class="form-label">ชื่อ *</label>
                    <input
                      type="text"
                      class="form-control"
                      :class="{ 'is-invalid': formErrors.firstName }"
                      v-model="bookingForm.firstName"
                      @blur="validateField('firstName')"
                      required
                    />
                    <div v-if="formErrors.firstName" class="invalid-feedback">
                      {{ formErrors.firstName }}
                    </div>
                  </div>
                  <div class="col-md-6">
                    <label class="form-label">นามสกุล *</label>
                    <input
                      type="text"
                      class="form-control"
                      :class="{ 'is-invalid': formErrors.lastName }"
                      v-model="bookingForm.lastName"
                      @blur="validateField('lastName')"
                      required
                    />
                    <div v-if="formErrors.lastName" class="invalid-feedback">
                      {{ formErrors.lastName }}
                    </div>
                  </div>
                  <div class="col-md-6">
                    <label class="form-label">อีเมล *</label>
                    <input
                      type="email"
                      class="form-control"
                      :class="{ 'is-invalid': formErrors.email }"
                      v-model="bookingForm.email"
                      @blur="validateField('email')"
                      required
                    />
                    <div v-if="formErrors.email" class="invalid-feedback">
                      {{ formErrors.email }}
                    </div>
                  </div>
                  <div class="col-md-6">
                    <label class="form-label">เบอร์โทรศัพท์ *</label>
                    <input
                      type="tel"
                      class="form-control"
                      :class="{ 'is-invalid': formErrors.phone }"
                      v-model="bookingForm.phone"
                      @blur="validateField('phone')"
                      placeholder="08xxxxxxxx"
                      maxlength="10"
                      required
                    />
                    <div v-if="formErrors.phone" class="invalid-feedback">
                      {{ formErrors.phone }}
                    </div>
                  </div>
                </div>
              </div>

              <div class="mb-4">
                <h5 class="fw-bold mb-3">วันที่เข้าพัก</h5>
                <div class="row g-3">
                  <div class="col-md-6">
                    <label class="form-label">เช็คอิน *</label>
                    <input
                      type="date"
                      class="form-control"
                      :class="{ 'is-invalid': formErrors.checkIn }"
                      v-model="bookingForm.checkIn"
                      :min="minDate"
                      @change="validateDates"
                      required
                    />
                    <div v-if="formErrors.checkIn" class="invalid-feedback">
                      {{ formErrors.checkIn }}
                    </div>
                  </div>
                  <div class="col-md-6">
                    <label class="form-label">เช็คเอาท์ *</label>
                    <input
                      type="date"
                      class="form-control"
                      :class="{ 'is-invalid': formErrors.checkOut }"
                      v-model="bookingForm.checkOut"
                      :min="bookingForm.checkIn"
                      @change="validateDates"
                      required
                    />
                    <div v-if="formErrors.checkOut" class="invalid-feedback">
                      {{ formErrors.checkOut }}
                    </div>
                  </div>
                  <div class="col-md-6">
                    <label class="form-label">จำนวนผู้เข้าพัก *</label>
                    <input
                      type="number"
                      class="form-control"
                      :class="{ 'is-invalid': formErrors.guests }"
                      v-model.number="bookingForm.guests"
                      @input="updateRoomsBasedOnGuests"
                      min="1"
                      required
                    />
                    <div v-if="formErrors.guests" class="invalid-feedback">
                      {{ formErrors.guests }}
                    </div>
                  </div>
                  <div class="col-md-6">
                    <label class="form-label">จำนวนห้อง</label>
                    <input
                      type="number"
                      class="form-control"
                      v-model.number="bookingForm.rooms"
                      min="1"
                      readonly
                    />
                  </div>
                </div>
              </div>

              <div class="mb-4">
                <h5 class="fw-bold mb-3">ข้อความพิเศษ</h5>
                <textarea
                  class="form-control"
                  rows="3"
                  v-model="bookingForm.specialRequests"
                  placeholder="ระบุความต้องการเพิ่มเติม (ไม่บังคับ)"
                  maxlength="500"
                ></textarea>
                <div class="form-text">{{ bookingForm.specialRequests.length }}/500 ตัวอักษร</div>
              </div>

              <div class="mb-4">
                <div class="form-check">
                  <input
                    class="form-check-input"
                    type="checkbox"
                    v-model="bookingForm.agreeTerms"
                    id="agree"
                    :class="{ 'is-invalid': formErrors.agreeTerms }"
                    @change="validateField('agreeTerms')"
                    required
                  />
                  <label class="form-check-label" for="agree">
                    ยอมรับ <a href="#" class="text-coral" @click.prevent="showTermsModal">เงื่อนไขและข้อกำหนด</a> *
                  </label>
                  <div v-if="formErrors.agreeTerms" class="invalid-feedback">
                    {{ formErrors.agreeTerms }}
                  </div>
                </div>
              </div>

              <button
                class="btn btn-coral w-100 py-2"
                @click="confirmBooking"
                :disabled="!canSubmit || submitting"
              >
                <span v-if="submitting">
                  <span class="spinner-border spinner-border-sm me-2" role="status"></span>
                  กำลังดำเนินการ...
                </span>
                <span v-else>
                  <i class="fas fa-check-circle me-2"></i>จองเลย
                </span>
              </button>
            </div>
          </div>
        </div>

        <div class="col-lg-4">
          <div class="card shadow-sm position-sticky" style="top: 20px;">
            <div class="card-header bg-coral text-white">
              <h5 class="fw-bold mb-0">สรุปการจอง</h5>
            </div>
            <div class="card-body summary-section">
              <div v-if="bookingData.hotel">
                <img :src="bookingData.hotel.image" class="img-fluid rounded mb-2" style="height: 150px; width: 100%; object-fit: cover;" :alt="bookingData.hotel.name" />
                <h6 class="fw-bold">{{ bookingData.hotel.name }}</h6>
                <p class="text-muted small mb-1">
                  <i class="fas fa-star text-warning"></i> {{ bookingData.hotel.rating }} / 10
                </p>
              </div>

              <div v-if="bookingData.room">
                <h6 class="fw-bold text-coral">{{ bookingData.room.name }}</h6>
                <p class="small text-muted mb-1">{{ bookingData.room.beds }}</p>
                <p class="small text-muted mb-3">{{ bookingData.room.guests }}</p>
              </div>

              <div v-if="bookingData.tours && bookingData.tours.length">
                <h6 class="fw-bold">ทัวร์ที่เลือก</h6>
                <div v-for="tour in bookingData.tours" :key="tour" class="d-flex justify-content-between small mb-1">
                  <span>{{ getTourName(tour) }}</span>
                  <span>{{ formatPrice(bookingData.tourPrices[tour]) }}</span>
                </div>
              </div>

              <div class="border-top pt-3 mt-3">
                <div class="d-flex justify-content-between">
                  <span>ค่าห้อง ({{ nights }} คืน)</span>
                  <span>{{ formatPrice(roomTotal) }}</span>
                </div>
                <div class="d-flex justify-content-between" v-if="tourTotal > 0">
                  <span>ทัวร์</span>
                  <span>{{ formatPrice(tourTotal) }}</span>
                </div>
                <hr />
                <div class="d-flex justify-content-between fw-bold fs-5">
                  <span>รวมทั้งหมด</span>
                  <span class="text-coral">{{ formatPrice(grandTotal) }}</span>
                </div>
              </div>

              <div class="mt-3 p-2 bg-light rounded">
                <small class="text-muted">
                  <i class="fas fa-calendar-check me-1"></i>
                  {{ formatDate(bookingForm.checkIn) }} - {{ formatDate(bookingForm.checkOut) }}
                </small>
                <br>
                <small class="text-muted">
                  <i class="fas fa-users me-1"></i>
                  {{ bookingForm.guests }} คน, {{ bookingForm.rooms }} ห้อง
                </small>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="modal fade" id="successModal" tabindex="-1" data-bs-backdrop="static" data-bs-keyboard="false">
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
          <div class="modal-header border-0 text-center">
            <div class="w-100">
              <i class="fas fa-check-circle text-success" style="font-size: 3rem;"></i>
              <h4 class="mt-2 mb-0">จองสำเร็จ!</h4>
            </div>
          </div>
          <div class="modal-body text-center">
            <p class="mb-1">หมายเลขการจอง:</p>
            <p class="fs-4 fw-bold text-coral mb-3">{{ bookingNumber }}</p>
            <p class="text-muted small">
              ข้อมูลการจองได้ถูกส่งไปยังอีเมล {{ bookingForm.email }} แล้ว
            </p>
            <img src="https://placehold.co/300x200/f0f0f0/cccccc?text=Booking+Confirmation" alt="Booking Confirmation" class="img-fluid my-3" />
          </div>
          <div class="modal-footer border-0 justify-content-center">
            <button class="btn btn-coral" @click="handleSuccessModalClose">ตกลง</button>
          </div>
        </div>
      </div>
    </div>

    <div class="modal fade" id="termsModal" tabindex="-1" aria-labelledby="termsModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-dialog-centered modal-lg">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="termsModalLabel">เงื่อนไขและข้อกำหนด</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <h6>1. การจองและการยืนยัน</h6>
            <p>การจองห้องพักจะถือว่าสมบูรณ์เมื่อผู้ใช้ได้รับอีเมลยืนยันการจองจาก Roomio เท่านั้น Roomio ขอสงวนสิทธิ์ในการยกเลิกการจองที่ไม่สมบูรณ์หรือมีข้อผิดพลาด</p>
            <h6>2. นโยบายการยกเลิก</h6>
            <p>
              การยกเลิกการจองจะต้องทำล่วงหน้าอย่างน้อย 24 ชั่วโมงก่อนเวลาเช็คอิน หากยกเลิกน้อยกว่า 24 ชั่วโมงหรือไม่ปรากฏตัว (No-Show) อาจมีค่าธรรมเนียมตามนโยบายของโรงแรมนั้นๆ
            </p>
            <h6>3. การชำระเงิน</h6>
            <p>
              ผู้ใช้จะต้องชำระเงินเต็มจำนวนตามที่ระบุไว้ในหน้าสรุปการจอง การชำระเงินสามารถทำได้ผ่านช่องทางที่ Roomio กำหนดไว้เท่านั้น
            </p>
            <h6>4. ข้อมูลส่วนบุคคล</h6>
            <p>
              Roomio จะเก็บรักษาข้อมูลส่วนบุคคลของผู้ใช้อย่างปลอดภัยและจะไม่เปิดเผยต่อบุคคลที่สามโดยไม่ได้รับอนุญาต ยกเว้นในกรณีที่จำเป็นตามกฎหมาย
            </p>
            <h6>5. ข้อจำกัดความรับผิดชอบ</h6>
            <p>
              Roomio ทำหน้าที่เป็นตัวกลางในการจองห้องพักเท่านั้น และจะไม่รับผิดชอบต่อความเสียหายหรือความไม่สะดวกใดๆ ที่เกิดจากการให้บริการของโรงแรมโดยตรง
            </p>
            <h6>6. การเปลี่ยนแปลงเงื่อนไข</h6>
            <p>
              Roomio ขอสงวนสิทธิ์ในการเปลี่ยนแปลงเงื่อนไขและข้อกำหนดเหล่านี้ได้ตลอดเวลาโดยไม่ต้องแจ้งให้ทราบล่วงหน้า
            </p>
          </div>
          <div class="modal-footer justify-content-center">
            <button type="button" class="btn btn-coral" @click="acceptTermsAndCloseModal">ยอมรับเงื่อนไข</button>
          </div>
        </div>
      </div>
    </div>

    <div class="modal fade" id="paymentModal" tabindex="-1" data-bs-backdrop="static" data-bs-keyboard="false">
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">เลือกช่องทางการชำระเงิน</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" @click="cancelPayment"></button>
          </div>
          <div class="modal-body text-center">
            <div class="d-grid gap-3">
              <button class="btn btn-outline-secondary py-3" @click="selectPaymentMethod('bankTransfer')">
                <i class="fas fa-university me-2"></i> โอนเงินผ่านธนาคาร
              </button>
              <button class="btn btn-outline-secondary py-3" @click="selectPaymentMethod('qrCode')">
                <i class="fas fa-qrcode me-2"></i> สแกน QR หรือพร้อมเพย์
              </button>
            </div>
            <div v-if="selectedPayment === 'bankTransfer'" class="mt-4 p-3 border rounded text-start">
              <p class="fw-bold mb-2">ข้อมูลบัญชีธนาคาร:</p>
              <p class="mb-1">ธนาคาร: กสิกรไทย</p>
              <p class="mb-1">ชื่อบัญชี: Roomio Co., Ltd.</p>
              <p class="mb-1">เลขที่บัญชี: 123-4-56789-0</p>
              <p class="mb-1">ประเภท: ออมทรัพย์</p>
              <button class="btn btn-coral mt-3 w-100" @click="processPayment">ยืนยันการชำระเงิน</button>
            </div>
            <div v-else-if="selectedPayment === 'qrCode'" class="mt-4 p-3 border rounded text-center">
              <p class="fw-bold mb-2">สแกน QR Code เพื่อชำระเงิน:</p>
              <img src="https://i.postimg.cc/nVvMy0Rk/image.png" alt="QR Code" class="img-fluid mb-3" />
              <p class="text-muted small">โปรดตรวจสอบยอดเงินและชื่อผู้รับก่อนยืนยัน</p>
              <button class="btn btn-coral mt-3 w-100" @click="processPayment">ยืนยันการชำระเงิน</button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="modal fade" id="errorModal" tabindex="-1" aria-labelledby="errorModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="errorModalLabel">เกิดข้อผิดพลาด!</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <p>{{ paymentErrorMessage }}</p>
          </div>
          <div class="modal-footer justify-content-center">
            <button type="button" class="btn btn-coral" data-bs-dismiss="modal">ตกลง</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import { useRoute, useRouter } from 'vue-router'

const router = useRouter()
const route = useRoute()

const loading = ref(true)
const error = ref('') 
const submitting = ref(false)
const selectedPayment = ref(null) 
const paymentErrorMessage = ref(''); 

const bookingData = ref({
  hotel: null,
  room: null,
  tours: [],
  tourPrices: {},
  totalPrice: 0
})

const bookingForm = ref({
  firstName: '',
  lastName: '',
  email: '',
  phone: '',
  checkIn: '',
  checkOut: '',
  guests: 1, 
  rooms: 1,  
  specialRequests: '',
  agreeTerms: false
})

const formErrors = ref({})
const bookingNumber = ref('')

let successModalInstance = null;
let termsModalInstance = null;
let paymentModalInstance = null;
let errorModalInstance = null;

const tourNames = {
  'phi-phi': 'ทัวร์เกาะไผ่, เกาะจีน และเกาะปีปอ',
  'zoo': 'ทัวร์สวนสัตว์ปีปอเขาเขียว',
  'cruise': 'ทัวร์สำราญท์ทะเลแบบสำราญ',
  'temple': 'ทัวร์เกาะชาม สัตบุน',
  'adventure': 'ทัวร์อิสสราย-เมืองเก่า',
  'spa': 'ทัวร์เรือยอร์ชต์ชมพระอาทิตย์ตก'
}

const minDate = computed(() => {
  return new Date().toISOString().split('T')[0]
})

const nights = computed(() => {
  if (!bookingForm.value.checkIn || !bookingForm.value.checkOut) return 1
  const inDate = new Date(bookingForm.value.checkIn)
  const outDate = new Date(bookingForm.value.checkOut)
  const diff = Math.ceil((outDate - inDate) / (1000 * 60 * 60 * 24))
  return diff > 0 ? diff : 1
})

const roomTotal = computed(() => {
  return (bookingData.value.room?.price || 0) * nights.value * bookingForm.value.rooms
})

const tourTotal = computed(() => {
  if (!bookingData.value.tours || !bookingData.value.tourPrices) return 0
  return bookingData.value.tours.reduce((sum, key) => sum + (bookingData.value.tourPrices[key] || 0), 0)
})

const grandTotal = computed(() => roomTotal.value + tourTotal.value)

const canSubmit = computed(() => {
  const f = bookingForm.value
  const noErrors = Object.keys(formErrors.value).every(key => !formErrors.value[key]);
  return f.firstName && f.lastName && f.email && f.phone &&
           f.checkIn && f.checkOut && f.agreeTerms &&
           f.guests > 0 && f.rooms > 0 &&
           noErrors;
})

watch(() => bookingForm.value.guests, (newGuests) => {
  updateRoomsBasedOnGuests(newGuests);
});

const loadBookingData = () => {
  try {
    const queryData = route.query.data
    if (queryData) {
      const parsed = JSON.parse(queryData)
      bookingData.value = parsed
      if (bookingData.value.room && bookingData.value.room.guests) {
        bookingForm.value.guests = bookingData.value.room.guests;
        updateRoomsBasedOnGuests(bookingForm.value.guests);
      }
    } else {
      throw new Error('ไม่พบข้อมูลการจอง')
    }
  } catch (e) {
    console.error('Error loading booking data:', e)
    error.value = 'ไม่สามารถโหลดข้อมูลการจองได้ กรุณาลองใหม่อีกครั้ง'
  }
}

const setDefaultDates = () => {
  const today = new Date()
  const tomorrow = new Date(today)
  tomorrow.setDate(today.getDate() + 1)
  const dayAfter = new Date(today)
  dayAfter.setDate(today.getDate() + 2)

  bookingForm.value.checkIn = tomorrow.toISOString().split('T')[0]
  bookingForm.value.checkOut = dayAfter.toISOString().split('T')[0]
}

const updateRoomsBasedOnGuests = (newGuests) => {
  const guests = parseInt(newGuests) || 0;
  if (guests > 0) {
    bookingForm.value.rooms = Math.ceil(guests / 3);
  } else {
    bookingForm.value.rooms = 1;
  }
  validateField('guests');
};

const validateField = (field) => {
  const form = bookingForm.value
  delete formErrors.value[field]

  switch (field) {
    case 'firstName':
    case 'lastName':
      if (!form[field]) {
        formErrors.value[field] = 'กรุณากรอกข้อมูลให้ครบถ้วน'
      } else if (form[field].length < 2) {
        formErrors.value[field] = 'ต้องมีอย่างน้อย 2 ตัวอักษร'
      }
      break
    case 'email':
      if (!form.email) {
        formErrors.value.email = 'กรุณากรอกอีเมล'
      } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email)) {
        formErrors.value.email = 'รูปแบบอีเมลไม่ถูกต้อง'
      }
      break
    case 'phone':
      if (!form.phone) {
        formErrors.value.phone = 'กรุณากรอกเบอร์โทรศัพท์'
      } else if (!/^\d{10}$/.test(form.phone)) {
        formErrors.value.phone = 'รูปแบบเบอร์โทรไม่ถูกต้อง (10 หลัก)'
      }
      break
    case 'guests':
      if (form.guests < 1) {
        formErrors.value.guests = 'จำนวนผู้เข้าพักต้องมีอย่างน้อย 1 คน';
      }
      break;
    case 'agreeTerms':
      if (!form.agreeTerms) {
        formErrors.value.agreeTerms = 'กรุณายอมรับเงื่อนไขและข้อกำหนด';
      }
      break;
  }
}

const validateDates = () => {
  const form = bookingForm.value
  delete formErrors.value.checkIn
  delete formErrors.value.checkOut

  if (!form.checkIn) {
    formErrors.value.checkIn = 'กรุณาเลือกวันเช็คอิน'
    return
  }

  if (!form.checkOut) {
    formErrors.value.checkOut = 'กรุณาเลือกวันเช็คเอาท์'
    return
  }

  const checkInDate = new Date(form.checkIn)
  const checkOutDate = new Date(form.checkOut)
  const today = new Date()
  today.setHours(0, 0, 0, 0)

  if (checkInDate < today) {
    formErrors.value.checkIn = 'ไม่สามารถเลือกวันที่ผ่านมาแล้ว'
  }

  if (checkOutDate <= checkInDate) {
    formErrors.value.checkOut = 'วันเช็คเอาท์ต้องอยู่หลังวันเช็คอิน'
  }
}

const validateAllFields = () => {
  validateField('firstName')
  validateField('lastName')
  validateField('email')
  validateField('phone')
  validateField('guests')
  validateDates()

  if (!bookingForm.value.agreeTerms) {
    formErrors.value.agreeTerms = 'กรุณายอมรับเงื่อนไขและข้อกำหนด'
  }
}

const handleSuccessModalClose = () => {
  if (successModalInstance) {
    successModalInstance.hide();
    document.getElementById('successModal').addEventListener('hidden.bs.modal', () => {
      router.push('/myBookings');
    }, { once: true });
  } else {
    router.push('/myBookings');
  }
};

const goBack = () => {
  router.go(-1)
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

const showTermsModal = () => {
  if (termsModalInstance) {
    termsModalInstance.show();
  }
};

const acceptTermsAndCloseModal = () => {
  bookingForm.value.agreeTerms = true;
  validateField('agreeTerms');
  if (termsModalInstance) {
    termsModalInstance.hide();
  }
};

const confirmBooking = async () => {
  validateAllFields()

  if (!canSubmit.value) {
    const firstErrorField = Object.keys(formErrors.value).find(key => formErrors.value[key]);
    if (firstErrorField) {
      const element = document.querySelector(`[name="${firstErrorField}"], [id="${firstErrorField}"]`);
      if (element) {
        element.scrollIntoView({ behavior: 'smooth', block: 'center' });
      } else {
        const firstInvalidElement = document.querySelector('.is-invalid');
        if (firstInvalidElement) {
          firstInvalidElement.scrollIntoView({ behavior: 'smooth', block: 'center' });
        }
      }
    }
    return
  }

  if (paymentModalInstance) {
    selectedPayment.value = null;
    paymentModalInstance.show();
  }
}

const selectPaymentMethod = (method) => {
  selectedPayment.value = method;
};

const cancelPayment = () => {
  if (paymentModalInstance) {
    paymentModalInstance.hide();
  }
  selectedPayment.value = null;
};

const processPayment = async () => {
  submitting.value = true;
  if (paymentModalInstance) {
    paymentModalInstance.hide();
  }

  try {
    await new Promise(resolve => setTimeout(resolve, 2000));

    bookingNumber.value = 'RM' + Date.now().toString().slice(-8);

    const bookingDetails = {
      bookingNumber: bookingNumber.value,
      hotel: bookingData.value.hotel,
      room: bookingData.value.room,
      tours: bookingData.value.tours,
      tourPrices: bookingData.value.tourPrices,
      customer: {
        firstName: bookingForm.value.firstName,
        lastName: bookingForm.value.lastName,
        email: bookingForm.value.email,
        phone: bookingForm.value.phone
      },
      dates: {
        checkIn: bookingForm.value.checkIn,
        checkOut: bookingForm.value.checkOut,
        nights: nights.value
      },
      guests: bookingForm.value.guests,
      rooms: bookingForm.value.rooms,
      specialRequests: bookingForm.value.specialRequests,
      pricing: {
        roomTotal: roomTotal.value,
        tourTotal: tourTotal.value,
        grandTotal: grandTotal.value
      },
      status: 'confirmed',
      bookingDate: new Date().toISOString(),
      paymentStatus: 'paid'
    };

    const existingBookings = JSON.parse(localStorage.getItem('userBookings') || '[]');
    existingBookings.push(bookingDetails);
    localStorage.setItem('userBookings', JSON.stringify(existingBookings));

    if (successModalInstance) {
      successModalInstance.show();
    }

  } catch (error) {
    console.error('Payment or Booking error:', error);
    paymentErrorMessage.value = 'เกิดข้อผิดพลาดในการชำระเงินหรือการจอง กรุณาลองใหม่อีกครั้ง';
    if (errorModalInstance) {
      errorModalInstance.show();
    }
  } finally {
    submitting.value = false;
  }
};

// Lifecycle
onMounted(async () => {
  try {
    loadBookingData()
    setDefaultDates()

    const { Modal } = await import('bootstrap');

    successModalInstance = new Modal(document.getElementById('successModal'));
    termsModalInstance = new Modal(document.getElementById('termsModal'));
    paymentModalInstance = new Modal(document.getElementById('paymentModal'));
    errorModalInstance = new Modal(document.getElementById('errorModal'));

    if (!bookingData.value.hotel) {
      throw new Error('ไม่พบข้อมูลโรงแรม')
    }

  } catch (err) {
    error.value = err.message || 'เกิดข้อผิดพลาดในการโหลดข้อมูล'
  } finally {
    loading.value = false
  }
})
</script>
