<script setup>
import { RouterView } from 'vue-router'

// Set page meta
useHead({
  title: 'Roomio - เว็บไซต์จองที่พักที่สะดวก ทันสมัย และน่าเชื่อถือ',
  meta: [
    { name: 'description', content: 'จองที่พักออนไลน์ง่ายๆ กับ Roomio พบที่พักคุณภาพดีในราคาที่คุ้มค่า ทั่วประเทศไทย' },
    { name: 'keywords', content: 'จองที่พัก, โรงแรม, รีสอร์ท, ที่พักพักผ่อน, ท่องเที่ยว, ประเทศไทย' },
    { name: 'viewport', content: 'width=device-width, initial-scale=1' },
    { name: 'theme-color', content: '#FD7A6D' }
  ]
})
</script>

<template>
  <div id="app">
    <NuxtPage />
  </div>
</template>

<style>
/* ========================================
    IMPORTS
======================================== */
@import url('https://fonts.googleapis.com/css2?family=Prompt:wght@300;400;500;600;700&display=swap');
@import 'https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css';
@import 'https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.css';

/* ========================================
    CSS VARIABLES (สีหลัก)
======================================== */
:root {
    --primary-color: #FD7A6D; /* สีหลัก Coral */
    --primary-hover: #FC5D4D; /* สีเมื่อ Hover */
    --text-color: #A37062;    /* สีข้อความหลัก */
    --icon-color: #A37062;    /* สีไอคอนหลัก */
    --background: #f8f9fa;
    --white: #ffffff;
    --border: #e2e8f0;
    --shadow: rgba(0, 0, 0, 0.1);
    --transition: all 0.3s ease;
    --navbar-height: 76px; /* ความสูงของ navbar */

    /* เพิ่มสีสำหรับสรุปการจอง */
    --summary-bg: #FFF3E0; /* สีพื้นหลังอ่อนๆ สำหรับสรุปการจอง */
    --summary-text: #E65100; /* สีข้อความเข้มขึ้นสำหรับสรุปการจอง */

    /* Custom color for Roomio application (ถูกย้ายมาจากด้านล่าง) */
    --coral: var(--primary-color); /* กำหนดให้ coral ชี้ไปที่ primary-color */
}

/* ========================================
    GLOBAL RESET & BASE STYLES
======================================== */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

html {
    scroll-behavior: smooth;
}

body {
    font-family: 'Prompt', sans-serif;
    line-height: 1.6;
    color: var(--text-color);
    background-color: var(--background);
    padding-top: var(--navbar-height); /* เพิ่ม padding-top สำหรับ fixed navbar */
    height: 100%; /* เพิ่มเพื่อให้ hero-section เต็มจอ */
}

#app {
    min-height: 100vh;
}

/* ========================================
    TYPOGRAPHY
======================================== */
h5, h6 {
    margin-bottom: 0.5rem;
    color: var(--text-color);
}

label {
    font-weight: 500;
    margin-bottom: 0;
    font-size: 0.95rem;
    color: var(--text-color);
}

.hero-title {
    font-size: 3.5rem; /* ใช้ขนาดที่ต้องการใน Hero Section */
    font-weight: 700;
    color: white; /* เปลี่ยนเป็นสีขาวเพื่อให้ตัดกับพื้นหลัง */
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.6); /* เพิ่มเงาให้ข้อความ */
}

.hero-subtitle {
    font-size: 1.5rem; /* ใช้ขนาดที่ต้องการใน Hero Section */
    line-height: 1.6;
    color: white; /* เปลี่ยนเป็นสีขาวเพื่อให้ตัดกับพื้นหลัง */
    text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.5); /* เพิ่มเงาให้ข้อความ */
}

.hotel-price {
    font-size: 1.25rem;
    font-weight: bold;
    color: var(--text-color); /* ใช้ตัวแปรสีข้อความ */
    text-align: end;
    line-height: 1.4;
}

/* ========================================
    COLORS & BRAND
======================================== */
.text-coral {
    color: var(--primary-color) !important; /* ใช้ตัวแปรสีหลัก */
}

.roomio-logo,
.navbar-brand {
    font-weight: 700;
    font-size: 1.5rem;
    color: var(--primary-color) !important; /* ใช้ตัวแปรสีหลัก */
}
.roomio-logo {
    font-family: 'Prompt', sans-serif; /* ใช้ Prompt เป็น default */
    font-weight: bold;
}


.text-gradient {
    background: linear-gradient(135deg, var(--primary-color), #ffd93d);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
}

/* ========================================
    BUTTONS
======================================== */
.btn-coral {
    background-color: var(--primary-color);
    border-color: var(--primary-color);
    color: var(--white);
    border-radius: 8px;
    font-weight: 500;
    transition: var(--transition);
    padding: 0.5rem 1.5rem;
}

.btn-coral:hover,
.btn-coral:focus,
.btn-coral:active {
    background-color: var(--primary-hover);
    border-color: var(--primary-hover);
    color: var(--white);
    transform: translateY(-1px);
    box-shadow: 0 4px 12px rgba(253, 122, 109, 0.3);
}

.btn-coral:focus {
    box-shadow: 0 0 0 0.2rem rgba(253, 122, 109, 0.25);
}

.btn-coral:disabled,
.btn-coral[disabled] {
    background-color: #cccccc;
    border-color: #cccccc;
    color: #666666;
    transform: none;
    box-shadow: none;
    cursor: not-allowed;
}

.btn-danger {
    background-color: #dc3545; /* ใช้สีแดงมาตรฐานของ Bootstrap */
    border-color: #dc3545;
    color: var(--white);
    border-radius: 2rem;
    font-weight: 500;
    transition: var(--transition);
    padding: 0.5rem 1.5rem;
}

.btn-danger:hover {
    background-color: #c82333;
    border-color: #bd2130;
    color: var(--white);
    transform: translateY(-1px);
    box-shadow: 0 4px 12px rgba(220, 53, 69, 0.3);
}


/* ========================================
    FORMS
======================================== */
.form-control,
.form-select {
    border-radius: 8px;
    border: 1px solid var(--border);
    font-size: 1rem;
    transition: var(--transition);
    padding: 0.75rem 1rem;
    color: var(--text-color);
}

.form-control:focus,
.form-select:focus,
.search-input:focus {
    border-color: var(--primary-color);
    box-shadow: 0 0 0 0.2rem rgba(253, 122, 109, 0.25);
}

.search-input {
    font-size: 1.1rem;
}

.input-group-text {
    border-radius: 8px;
    border: 1px solid #dee2e6;
    background-color: var(--background);
    transition: var(--transition);
    padding: 0.75rem 1rem;
    border-right: 0;
}

.input-group .form-control {
    border-left: 0;
    border-radius: 0 10px 10px 0;
}

.guest-dropdown .input-group:hover {
    cursor: pointer;
}

.guest-dropdown .input-group:hover .input-group-text,
.guest-dropdown .input-group:hover .form-control {
    border-color: var(--primary-color);
}

.form-check-input:checked {
    background-color: var(--primary-color);
    border-color: var(--primary-color);
}

.form-check-label {
    font-size: 0.95rem;
    cursor: pointer;
    user-select: none;
}

.form-check {
    margin: 0;
}

.form-check-input {
    margin-top: 0;
    cursor: pointer;
}

/* ========================================
    LAYOUT & CONTAINERS
======================================== */
/* NOTE: home-container และ hero-section styles ถูกย้ายมาที่นี่ */
.home-container {
  /* ไม่ต้องใส่ background ที่นี่แล้ว เพราะ hero-section จะจัดการเอง */
  min-height: 100vh;
  position: relative; /* Added for pseudo-element to work correctly */
}

.hero-section {
  position: relative;
  width: 100%;
  height: 100vh; /* ทำให้ Hero Section เต็มความสูงของ viewport */
  display: flex;
  align-items: center; /* จัดฟอร์มให้อยู่ตรงกลางแนวตั้ง */
  justify-content: center; /* จัดฟอร์มให้อยู่ตรงกลางแนวนอน */
  overflow: hidden; /* ซ่อนส่วนเกินของภาพพื้นหลัง */
  padding-top: var(--navbar-height); /* ชดเชยความสูงของ fixed navbar */
}

.hero-section::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('https://lh4.googleusercontent.com/vIpnncldIGBH6ddc_jHjvl9c5kN9hBTHcA8EN2bOVgaAq0xubt_AlWjalHBfNR3rZtz6PRsKALHzTz9z4W_O7uiMnhrDPIMiwsQZV5JoA44W3sbhHsYmrXjyQae3HRFsvRZiHXG1TV-qQvHfE2iCJwo'); /* URL รูปภาพที่คุณให้มา */
  background-size: cover; /* ครอบคลุมพื้นที่ทั้งหมด */
  background-position: center; /* จัดตำแหน่งกึ่งกลางภาพ */
  background-repeat: no-repeat; /* ไม่ให้ซ้ำรูปภาพ */
  opacity: 0.4; /* ปรับความทึบแสงให้เหลือ 40% */
  z-index: -1; /* วางภาพพื้นหลังไว้ด้านหลังเนื้อหา */
}

/* New background for hotels.vue */
.hotels-page-background::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-image: url('https://i1.wp.com/th.blog.kkday.com/wp-content/uploads/Tailand_Koh-Lipe_shutterstock_547231402.jpg?fit=1290%2C860&ssl=1'); /* New image URL */
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    opacity: 0.4; /* Same intensity as index.vue hero-section */
    z-index: -1;
}


.search-form-container {
    max-width: 700px; /* จำกัดความกว้างของฟอร์มค้นหา */
    margin: 0 auto; /* จัดกึ่งกลางฟอร์ม */
    padding: 30px !important; /* เพิ่ม padding ในฟอร์ม */
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.2);
}

.search-results-section {
    background-color: var(--white);
    min-height: 300px;
}

.facilities-container {
    border: none;
    background-color: transparent;
    padding: 24px;
    min-height: 250px;
    margin-bottom: 1.5rem;
    flex: 0 0 70%;
    max-width: 70%;
    font-size: 1rem;
    font-weight: 400;
    line-height: 1.5;
    color: var(--text-color);
}

.nearby-container {
    border: 1px solid var(--border); /* ใช้ตัวแปรสีขอบ */
    border-radius: 12px;
    padding: 24px;
    background-color: var(--white);
    min-height: 250px;
    margin-bottom: 1.5rem;
    flex: 0 0 28%;
    max-width: 28%;
    margin-left: auto;
    font-size: 1rem;
    font-weight: 400;
    line-height: 1.5;
    color: var(--text-color); /* ใช้ตัวแปรสีข้อความ */
}

.nearby-container ul {
    list-style: none;
    padding-left: 0;
}

.nearby-container li {
    margin-bottom: 12px;
    font-weight: 500;
    display: flex;
    align-items: center;
    gap: 8px;
    color: var(--text-color) !important; /* ทำให้แน่ใจว่าใช้สีตัวแปร */
}

.nearby-container i {
    color: var(--icon-color) !important; /* ใช้ตัวแปรสีไอคอน */
    font-size: 1.2rem;
    margin-right: 0.5rem; /* เดิมเป็น gap: 0.5rem แต่ที่นี่ควรเป็น margin-right */
}


.row.align-items-start {
    display: flex;
    justify-content: space-between;
    gap: 1.5rem;
}

.room-detail-container {
    margin-top: 2rem;
    gap: 1.5rem;
}

.room-image-frame {
    /* สไตล์ถูกย้ายมาอยู่ด้านล่างเพื่อให้รวมกับ .room-card-new */
}

.room-info-frame {
    border: 1px solid var(--border); /* ใช้ตัวแปรสีขอบ */
    background-color: var(--white);
    min-height: 250px;
}

/* ========================================
    NAVIGATION - FIXED NAVBAR
======================================== */
.navbar {
    padding: 1rem 0;
    backdrop-filter: blur(10px);
    transition: var(--transition);
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1030;
    background-color: rgba(255, 255, 255, 0.95) !important;
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
}

.navbar.scrolled {
    background-color: rgba(255, 255, 255, 0.98) !important;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.nav-link {
    font-weight: 500;
    color: var(--text-color) !important; /* ใช้ตัวแปรสีข้อความ */
    transition: var(--transition);
    padding: 0.5rem 1rem !important;
    border-radius: 6px;
}

.nav-link:hover {
    color: var(--primary-color) !important; /* ใช้ตัวแตรสีหลัก */
    background-color: rgba(163, 112, 98, 0.1); /* ใช้สีที่เข้ากับ text-color */
}

.nav-link.text-coral {
    color: var(--primary-color) !important; /* ใช้ตัวแปรสีหลัก */
    background-color: rgba(163, 112, 98, 0.1); /* ใช้สีที่เข้ากับ text-color */
}

/* ========================================
    CARDS
======================================== */
.card {
    border-radius: 16px;
    overflow: hidden;
    transition: var(--transition);
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
    border: none;
}

.card:hover {
    transform: translateY(-4px);
    box-shadow: 0 6px 18px var(--shadow);
}

.card-img-top {
    width: 100%;
    height: 180px;
    object-fit: cover;
    border-bottom: 1px solid #f1f1f1;
}

.card-body {
    padding: 16px;
}

.card-body h6 {
    font-size: 1rem;
    font-weight: 600;
    color: var(--text-color);
    margin-bottom: 0.25rem;
}

.card-body p {
    margin: 0;
    font-size: 0.9rem;
    color: var(--text-color);
}

.card-body .text-danger {
    font-weight: 600;
    color: var(--primary-color); /* ใช้ตัวแปรสีหลัก */
}

.room-card { /* สไตล์นี้ควรถูกแทนที่ด้วย .room-card-new */
    transition: var(--transition);
    background-color: var(--white);
    border: 1px solid #eaeaea;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
    border-radius: 12px;
    padding: 1rem;
    margin-bottom: 1.5rem;
}

.room-card:hover { /* สไตล์นี้ควรถูกแทนที่ด้วย .room-card-new:hover */
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.08);
    transform: translateY(-3px);
    border-color: var(--primary-color);
}

.room-card img {
    border-radius: 10px;
}

.room-card .room-info h5 {
    font-size: 1.2rem;
    font-weight: 600;
    margin-bottom: 0.75rem;
    color: var(--text-color); /* ใช้ตัวแปรสีข้อความ */
}

.room-card .room-detail {
    font-size: 0.95rem;
    color: #555;
}

.room-card .icon-text {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    margin-bottom: 0.5rem;
}

.room-card .icon-text i {
    font-size: 1.2rem;
    color: var(--primary-color); /* ใช้ตัวแปรสีหลัก */
}

.room-card .price {
    font-size: 1.25rem;
    font-weight: bold;
    color: var(--text-color); /* ใช้ตัวแปรสีข้อความ */
}

.room-card .btn-coral {
    padding: 0.5rem 1.25rem;
    border-radius: 50px;
    font-weight: 500;
    font-size: 0.95rem;
}

/* ========================================
    IMAGES & GALLERY
======================================== */
.hotel-main-image {
    height: 400px;
    object-fit: cover;
    width: 100%;
    border-radius: 16px;
}

.hotel-sub-image {
    width: 140px;
    height: 90px;
    object-fit: cover;
    border-radius: 8px;
    transition: transform 0.2s;
}

.hotel-sub-image:hover {
    transform: scale(1.03);
}

.hotel-thumbnail {
    height: 100px;
    object-fit: cover;
    border-radius: 12px;
    cursor: pointer;
    border: 2px solid transparent;
    transition: border 0.2s;
}

.hotel-thumbnail:hover {
    border-color: var(--primary-color);
}

.hotel-gallery {
    display: grid;
    grid-template-columns: 2fr 1fr;
    gap: 8px;
    height: 400px;
    border-radius: 16px;
    overflow: hidden;
    /* เพิ่มเงาและขอบให้กับ Hotel Gallery */
    box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15); /* Stronger shadow */
    border: 1px solid var(--border); /* Add a subtle border */
}

.main-image {
    height: 100%;
    overflow: hidden;
}

.main-image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.3s ease;
}

.main-image:hover img {
    transform: scale(1.05);
}

.sub-images {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px;
    height: 100%;
}

.sub-image {
    position: relative;
    height: 100%;
    overflow: hidden;
    cursor: pointer;
}

.sub-image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.3s ease;
}

.sub-image:hover img {
    transform: scale(1.05);
}

.last-image {
    position: relative;
}

.image-overlay {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.6);
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--white);
    font-weight: 600;
    font-size: 1.1rem;
    opacity: 0;
    transition: opacity 0.3s ease;
}

.last-image:hover .image-overlay {
    opacity: 1;
}

.overlay-text {
    text-align: center;
    padding: 10px;
}

.img-fluid {
    border-radius: 1rem;
}

/* ========================================
    ICONS & DETAILS
======================================== */
.detail-icon {
    background-color: var(--white);
    border: 1px solid var(--border); /* ใช้ตัวแปรสีขอบ */
    padding: 14px 12px;
    border-radius: 12px;
    text-align: center;
    font-weight: 500;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 6px;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.03);
    height: 100%;
    transition: var(--transition);
    cursor: default;
}

.detail-icon i {
    font-size: 20px;
    color: var(--primary-color); /* ใช้ตัวแปรสีหลัก */
}

.detail-icon:hover {
    background-color: var(--background);
    transform: translateY(-1px);
}

.room-info-frame i,
.d-flex.gap-2 > i {
    color: #666;
}

/* ========================================
    DROPDOWNS
======================================== */
.dropdown-panel {
    animation: slideDown 0.3s ease;
}

@keyframes slideDown {
    from {
        opacity: 0;
        transform: translateY(-10px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.rotate-180 {
    transform: rotate(180deg);
    transition: transform 0.3s ease;
}

.dropdown-menu {
    border-radius: 12px;
    border: none;
    box-shadow: 0 10px 40px var(--shadow);
    padding: 0.5rem;
}

.dropdown-item {
    border-radius: 8px;
    font-weight: 500;
    color: var(--text-color); /* ใช้ตัวแปรสีข้อความ */
    transition: all 0.2s ease;
}

.dropdown-item:hover {
    background-color: rgba(253, 122, 109, 0.1); /* ใช้สีที่เข้ากับ primary-color */
    color: var(--primary-color);
}

/* ========================================
    MODALS & COMPONENTS
======================================== */
.modal-content {
    border-radius: 16px;
    border: none;
    box-shadow: 0 10px 40px var(--shadow);
}

.modal-header {
    border-bottom: 1px solid var(--border);
    border-radius: 16px 16px 0 0;
}

.modal-footer {
    border-top: 1px solid var(--border);
    border-radius: 0 0 16px 16px;
}

.toast {
    border-radius: 12px;
    border: none;
    box-shadow: 0 8px 25px var(--shadow);
}

.toast-header {
    background-color: var(--primary-color);
    color: var(--white);
    border-radius: 12px 12px 0 0;
}

.badge {
    border-radius: 12px;
    font-weight: 500;
    padding: 0.5rem 0.75rem;
}

.badge-coral {
    background-color: var(--primary-color);
    color: var(--white);
}

.progress {
    border-radius: 10px;
    background-color: #f1f5f9;
}

.progress-bar {
    border-radius: 10px;
}

.progress-bar-coral {
    background-color: var(--primary-color);
}

.table {
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
}

.table thead th {
    background-color: var(--background);
    border-bottom: 2px solid var(--border);
    font-weight: 600;
    color: #374151;
}

.alert {
    border-radius: 12px;
    border: none;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
}

.alert-coral {
    background-color: rgba(253, 122, 109, 0.1);
    color: var(--primary-color); /* ใช้ตัวแปรสีหลัก */
    border-left: 4px solid var(--primary-color);
}

/* ========================================
    UTILITIES
======================================== */
.cursor-pointer {
    cursor: pointer;
}

.user-select-none {
    user-select: none;
}

.transition-all {
    transition: var(--transition);
}

.spinner-coral {
    color: var(--primary-color);
}

.list-unstyled li {
    margin-bottom: 10px;
}

.visually-hidden-focusable:not(:focus):not(:focus-within) {
    position: absolute !important;
    width: 1px !important;
    height: 1px !important;
    padding: 0 !important;
    margin: -1px !important;
    overflow: hidden !important;
    clip: rect(0, 0, 0, 0) !important;
    white-space: nowrap !important;
    border: 0 !important;
}

.room-card .room-price {
    width: 100%;
    text-align: end;
    margin-top: 1rem;
}

/* ========================================
    RESPONSIVE DESIGN
======================================== */
@media (max-width: 768px) {
    body {
        padding-top: 70px; /* ลด padding สำหรับมือถือ */
    }

    .hero-section {
        padding-top: 60px;
        height: auto; /* ปรับให้ความสูงเป็น auto บนมือถือเพื่อรองรับเนื้อหา */
        min-height: 100vh; /* ให้ยังคงสูงเต็มจอเมื่อเนื้อหาน้อย */
    }

    .hero-title {
        font-size: 2.5rem;
    }

    .hero-subtitle {
        font-size: 1.2rem;
    }

    .search-form-container {
        margin: 0 15px;
    }

    .card-img-top,
    .hotel-thumbnail {
        height: 150px;
    }

    .hotel-thumbnail {
        height: 70px;
    }

    .display-1 {
        font-size: 2.5rem;
    }

    .display-2 {
        font-size: 2rem;
    }

    .display-3 {
        font-size: 1.75rem;
    }

    .display-4 {
        font-size: 1.5rem;
    }

    .facilities-container,
    .nearby-container {
        flex: 0 0 100%;
        max-width: 100%;
        margin-bottom: 1.5rem;
        min-height: auto;
    }

    .room-card {
        text-align: center;
    }

    .room-card .col-md-3,
    .room-card .col-md-4,
    .room-card .col-md-5 {
        margin-top: 1rem;
        text-align: center;
    }

    .room-card .text-end {
        text-align: center !important;
        margin-top: 1rem;
    }

    .room-card .icon-text {
        justify-content: center;
    }

    .hotel-gallery {
        grid-template-columns: 1fr;
        height: auto;
        gap: 12px;
    }

    .main-image {
        height: 250px;
    }

    .sub-images {
        grid-template-columns: repeat(2, 1fr);
        height: 120px;
    }

    .sub-image {
        height: 120px;
    }
}

@media (max-width: 576px) {
    .hero-title {
        font-size: 1.8rem;
    }

    .hero-section {
        padding: 40px 0;
        padding-top: 50px;
    }

    .display-1 {
        font-size: 2rem;
    }

    .display-2 {
        font-size: 1.75rem;
    }

    .display-3 {
        font-size: 1.5rem;
    }

    .display-4 {
        font-size: 1.25rem;
    }

    .hotel-gallery {
        gap: 8px;
    }

    .main-image {
        height: 200px;
    }

    .sub-images {
        height: 100px;
    }

    .sub-image {
        height: 100px;
    }
}

/* ========================================
    ACCESSIBILITY & SPECIAL FEATURES
======================================== */
@media (prefers-color-scheme: dark) {
    .auto-dark {
        background-color: #1a202c;
        color: #f7fafc;
    }
}

@media (prefers-contrast: high) {
    .btn-coral {
        border: 2px solid #000;
    }

    .form-control {
        border: 2px solid #000;
    }
}

@media (prefers-reduced-motion: reduce) {
    * {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
    }
}

@media print {
    .no-print {
        display: none !important;
    }

    .navbar {
        display: none !important;
    }

    body {
        padding-top: 0 !important;
    }
}

/* Additional utility classes */
/* ใช้ตัวแปรสีที่กำหนดไว้ใน :root */
.bg-coral {
    background-color: var(--primary-color) !important; /* ใช้ primary-color แทน coral */
}

.booking-container {
    min-height: 100vh;
    background-color: var(--background);
}

.booking-card {
    background: var(--white);
    border-radius: 15px;
    box-shadow: var(--shadow); /* ใช้ตัวแปร shadow */
    overflow: hidden;
    margin-top: -50px;
    position: relative;
    z-index: 10;
}

/* Animation Classes */
.fade-in {
    animation: fadeIn 0.6s ease-in;
}

@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.slide-up {
    animation: slideUp 0.8s ease-out;
}

@keyframes slideUp {
    from {
        opacity: 0;
        transform: translateY(50px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

/* Loading States */
.loading-shimmer {
    background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
    background-size: 200% 100%;
    animation: shimmer 1.5s infinite;
}

@keyframes shimmer {
    0% {
        background-position: -200% 0;
    }
    100% {
        background-position: 200% 0;
    }
}

/* Styles for summary section to ensure contrast */
.card-header.bg-coral {
    background-color: var(--primary-color) !important;
    color: var(--white) !important;
}

.summary-section {
    background-color: var(--summary-bg); /* ใช้สีพื้นหลังที่ตัดกัน */
    color: var(--summary-text); /* ใช้สีข้อความที่ตัดกัน */
    padding: 1rem;
    border-radius: 12px;
}

.summary-section h5,
.summary-section h6,
.summary-section span {
    color: var(--summary-text); /* ให้ข้อความเป็นสีที่ตัดกัน */
}

/* MyBookings page specific styles */
.my-bookings-container {
    position: relative; /* Add position relative for pseudo-element */
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-top: var(--navbar-height); /* Ensure content is below navbar */
    overflow: hidden; /* Ensure content doesn't overflow the pseudo-element */
}

.my-bookings-container::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-image: url('https://placehold.co/1920x1080/e0e0e0/cccccc?text=Background'); /* Placeholder image */
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    opacity: 0.4; /* Same intensity as index.vue */
    z-index: -1; /* Place behind content */
}

/* Style for navbar brand image */
.navbar-brand img {
    border-radius: 50%; /* ทำให้รูปภาพโลโก้เป็นวงกลม */
}

.booking-item-card {
    border-radius: 16px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.08);
    transition: var(--transition);
    border: none;
}

.booking-item-card:hover {
    transform: translateY(-3px);
    box-shadow: 0 6px 20px rgba(0,0,0,0.12);
}

.booking-item-card .card-img-top {
    height: 200px;
    object-fit: cover;
    border-radius: 16px 16px 0 0;
}

.booking-item-card .booking-status {
    font-weight: 600;
    padding: 0.3rem 0.8rem;
    border-radius: 20px;
    font-size: 0.85rem;
}

.status-confirmed {
    background-color: #d4edda;
    color: #155724;
}

.status-pending {
    background-color: #fff3cd;
    color: #856404;
}

.status-cancelled {
    background-color: #f8d7da;
    color: #721c24;
}

.image-modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.85);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 2000;
    cursor: pointer;
}

.modal-image {
    max-width: 90vw;
    max-height: 90vh;
    object-fit: contain;
    border-radius: 8px;
    cursor: default;
}

.close-modal-btn {
    position: absolute;
    top: 20px;
    right: 30px;
    font-size: 3rem;
    color: white;
    font-weight: bold;
    cursor: pointer;
    line-height: 1;
}

/* NEW: Hotel Price Detail Style */
.hotel-price-detail {
    font-size: 1rem;
    font-weight: normal;
    color: #6c757d; /* text-muted color */
    line-height: 1.4;
}

.hotel-price-detail .fs-4 {
    color: var(--text-color, #A37062);
}


/* UPDATED: Room Card Styles for clear separation */
.room-card-new {
    /* This is now a simple wrapper for alignment */
    transition: all 0.3s ease;
}

.room-image-frame {
    height: 100%;
    border-radius: 16px;
    overflow: hidden;
    /* เพิ่มเงาและขอบให้กับ Room Image Frame */
    box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15); /* Stronger shadow */
    border: 1px solid var(--border); /* Add a subtle border */
    transition: all 0.3s ease;
}

.room-image-frame:hover {
    transform: translateY(-3px) scale(1.02); /* Slightly more pronounced lift and scale */
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.2); /* Even stronger shadow on hover */
}

.room-image-frame img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.room-details-frame {
    padding: 1.5rem;
    height: 100%;
    display: flex;
    flex-direction: column;
    border: 1px solid #e9ecef;
    border-radius: 16px;
    background-color: #fff;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.07);
    transition: all 0.3s ease;
}

/* UPDATED: Selected Room Indication */
.selected-room .room-details-frame {
    border: 2px solid var(--primary-color, #FD7A6D);
    box-shadow: 0 6px 20px rgba(253, 122, 109, 0.3);
}


/* NEW: Tour Item Bordered Style */
.tour-item-bordered {
    border: 1px solid #e0e0e0;
    border-radius: 12px;
    background-color: #ffffff;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
    transition: all 0.3s ease;
}

.tour-item-bordered:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
}

/* Responsive adjustments for new room card */
@media (max-width: 991.98px) {
    .room-card-new .text-end {
        text-align: left !important;
        align-items: flex-start !important;
        margin-top: 1rem;
    }
}

/* Styles for summary section */
.summary-section img {
    border-radius: 8px; /* ขอบโค้งมนสำหรับรูปภาพในสรุป */
}
</style>
