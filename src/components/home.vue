<template>
  <div>
    <section id="home" class="min-vh-100 position-relative">
      <div
        id="spaCarousel"
        class="carousel slide"
        data-bs-ride="carousel"
        data-bs-interval="5000"
      >
        <!-- Carousel Indicators -->
        <div class="carousel-indicators">
          <button
            v-for="(slide, index) in slides"
            :key="index"
            type="button"
            :data-bs-target="'#spaCarousel'"
            :data-bs-slide-to="index"
            :class="{ active: index === currentSlide }"
            :aria-current="index === currentSlide ? 'true' : 'false'"
            :aria-label="'Slide ' + (index + 1)"
          ></button>
        </div>

        <!-- Carousel Items -->
        <div class="carousel-inner">
          <div
            v-for="(slide, index) in slides"
            :key="slide.title"
            class="carousel-item"
            :class="{ active: index === currentSlide }"
            :style="{
              backgroundImage: `linear-gradient(rgba(0, 0, 0, 0.5), rgba(120, 186, 126, 0.3)), url(${slide.image})`,
            }"
          >
            <div
              class="carousel-caption d-flex flex-column justify-content-center h-100"
            >
              <h1
                class="display-2 fw-bold mb-4 font-lora animate__animated animate__fadeInDown"
              >
                Chào mừng đến TutaSpa
              </h1>
              <h2
                class="fs-3 fw-semibold text-white mb-4 animate__animated animate__fadeInUp animate__delay-1s"
              >
                {{ slides[currentSlide].subtitle }}
              </h2>
              <p
                class="lead text-white mb-5 mx-auto animate__animated animate__fadeInUp animate__delay-2s"
                style="max-width: 700px"
              >
                Hành trình thư giãn và tái tạo năng lượng với những liệu pháp tự
                nhiên, mang đến sự cân bằng hoàn hảo cho cơ thể và tâm hồn bạn
              </p>
              <div
                class="d-flex flex-wrap justify-content-center gap-4 mb-5 animate__animated animate__fadeInUp animate__delay-3s"
              >
                <div
                  v-for="(stat, index) in stats"
                  :key="index"
                  class="text-center px-3"
                >
                  <div class="fs-1 mb-2">{{ stat.icon }}</div>
                  <div class="fs-3 fw-bold text-warning">{{ stat.number }}</div>
                  <div class="text-white">{{ stat.label }}</div>
                </div>
              </div>
              <div
                class="d-flex flex-column flex-sm-row justify-content-center gap-3 animate__animated animate__fadeInUp animate__delay-4s"
              >
                <router-link
                  to="/#services"
                  class="btn btn-primary btn-lg rounded-pill px-5 py-3 fw-bold"
                >
                  Khám phá dịch vụ
                </router-link>
                <router-link
                  to="/#booking"
                  class="btn btn-outline-light btn-lg rounded-pill px-5 py-3"
                >
                  Đặt lịch ngay
                </router-link>
              </div>
            </div>
          </div>
        </div>

        <!-- Carousel Controls -->
        <button
          class="carousel-control-prev"
          type="button"
          data-bs-target="#spaCarousel"
          data-bs-slide="prev"
        >
          <span class="carousel-control-prev-icon" aria-hidden="true"></span>
          <span class="visually-hidden">Previous</span>
        </button>
        <button
          class="carousel-control-next"
          type="button"
          data-bs-target="#spaCarousel"
          data-bs-slide="next"
        >
          <span class="carousel-control-next-icon" aria-hidden="true"></span>
          <span class="visually-hidden">Next</span>
        </button>
      </div>
    </section>

    <!-- Features Section -->
    <section class="features">
      <div class="container">
        <div class="features-grid">
          <div class="feature-card">
            <div class="feature-icon">🌱</div>
            <h3>100% Tự nhiên</h3>
            <p>
              Sử dụng các sản phẩm từ thiên nhiên, không chất hóa học có hại, an
              toàn cho mọi loại da
            </p>
          </div>
          <div class="feature-card">
            <div class="feature-icon">👥</div>
            <h3>Chuyên gia giàu kinh nghiệm</h3>
            <p>
              Đội ngũ chuyên viên được đào tạo bài bản, và có kinh nghiệm hơn 5
              năm
            </p>
          </div>
          <div class="feature-card">
            <div class="feature-icon">🛏️</div>
            <h3>Không gian sang trọng</h3>
            <p>
              Thiết kế hiện đại, thoáng mát với âm nhạc thư giãn và hương thơm
              dễ chịu
            </p>
          </div>
          <div class="feature-card">
            <div class="feature-icon">🛡️</div>
            <h3>Đảm bảo vệ sinh</h3>
            <p>
              Tuân thủ nghiêm ngặt các tiêu chuẩn vệ sinh, khử trùng dụng cụ sau
              mỗi lần sử dụng
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- Services Section -->
    <section id="services" class="services">
      <div class="container">
        <h2 class="section-title">Dịch vụ của chúng tôi</h2>
        <p class="section-subtitle">
          Trải nghiệm những dịch vụ chăm sóc sức khỏe và làm đẹp hàng đầu với
          công nghệ hiện đại và nguyên liệu tự nhiên
        </p>

        <div class="service-categories">
          <button
            class="category-btn"
            :class="{ active: currentCategory === 'all' }"
            @click="filterServices('all')"
          >
            Tất cả
          </button>
          <button
            v-for="category in categories"
            :key="category.loaiDichVuID"
            class="category-btn"
            :class="{ active: currentCategory === category.tenLoai }"
            @click="filterServices(category.tenLoai)"
          >
            {{ category.tenLoai }}
          </button>
        </div>

        <div v-if="loading" class="loading">
          <div class="loading-spinner"></div>
          <p>Đang tải dịch vụ...</p>
        </div>
        <div v-else-if="error" class="error">
          <p>{{ error }}</p>
        </div>
        <div v-else class="services-grid">
          <div
            v-for="service in filteredServices"
            :key="service.id"
            class="service-card"
          >
            <div class="service-image-container">
              <router-link
                class="detail-service-link"
                :to="`/DichVuChiTiet/${service.id}`"
              >
                <img
                  :src="service.image"
                  :alt="service.name"
                  class="service-image"
                />
              </router-link>
              <div class="service-rating-overlay">
                <div class="service-rating">
                  <i
                    v-for="n in 5"
                    :key="n"
                    class="rating-star"
                    :class="
                      n <= Math.floor(service.mucDanhGia)
                        ? 'filled'
                        : n - 0.5 <= service.mucDanhGia
                        ? 'half-filled'
                        : ''
                    "
                    >★</i
                  >
                  <span class="rating-text"
                    >({{ service.mucDanhGia?.toFixed(1) || "0.0" }})</span
                  >
                </div>
              </div>
            </div>
            <div class="service-content">
              <router-link
                class="detail-service-link"
                :to="`/DichVuChiTiet/${service.id}`"
              >
                <h3 class="service-title">{{ service.name }}</h3>
                <div class="service-meta">
                  <div class="service-duration">
                    <i class="duration-icon">⏱</i>
                    {{ service.duration }} phút
                  </div>
                  <div class="service-price">{{ service.price }} VNĐ</div>
                </div>
              </router-link>
              <!-- <p class="service-description">{{ service.description }}</p> -->
              <button
                class="service-book-btn"
                @click="addServiceFromCard(service)"
              >
                <span>Đặt lịch ngay</span>
                <i class="btn-arrow">→</i>
              </button>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Improved Testimonials Section -->
    <section class="testimonials-section">
      <div class="container">
        <h2 class="section-title text-center mb-5">
          Khách hàng nói gì về chúng tôi
        </h2>

        <div v-if="loading" class="text-center text-white">
          <div class="loading-spinner"></div>
          <p>Đang tải đánh giá...</p>
        </div>

        <div
          v-else-if="testimonials.length === 0"
          class="text-center text-white opacity-75"
        >
          <div class="no-reviews-icon">💬</div>
          <p class="mt-3">
            Chưa có đánh giá nào. Hãy là người đầu tiên chia sẻ trải nghiệm của
            bạn!
          </p>
        </div>

        <div v-else class="testimonials-container">
          <!-- Always visible testimonials (first 3) -->
          <div class="testimonials-grid">
            <div
              v-for="(item, index) in visibleTestimonials"
              :key="index"
              class="testimonial-card"
              :class="{ 'testimonial-featured': index === 0 }"
            >
              <div class="testimonial-content">
                <div class="testimonial-header">
                  <div class="testimonial-avatar">
                    <span>{{ getInitials(item.name || "Khách hàng") }}</span>
                  </div>
                  <div class="testimonial-info">
                    <h4 class="testimonial-name">
                      {{ item.name || "Khách hàng" }}
                    </h4>
                    <div class="testimonial-rating">
                      <i
                        v-for="n in 5"
                        :key="n"
                        class="fa-star fas"
                        :class="
                          n <= item.rate
                            ? 'text-warning'
                            : 'text-secondary opacity-25'
                        "
                      ></i>
                    </div>
                  </div>
                </div>
                <p class="testimonial-text">
                  "{{ item.content || "Dịch vụ tuyệt vời, tôi rất hài lòng!" }}"
                </p>
              </div>
            </div>
          </div>

          <!-- Show more button if there are more than 3 testimonials -->
          <div v-if="testimonials.length > 3" class="text-center mt-4">
            <button class="show-more-btn" @click="toggleShowAll">
              <span v-if="!showAllTestimonials">
                Xem thêm {{ testimonials.length - 3 }} đánh giá khác
                <i class="fas fa-chevron-down ml-2"></i>
              </span>
              <span v-else>
                Thu gọn
                <i class="fas fa-chevron-up ml-2"></i>
              </span>
            </button>
          </div>

          <!-- Additional testimonials (hidden by default) -->
          <div
            v-if="showAllTestimonials && testimonials.length > 3"
            class="additional-testimonials"
          >
            <div class="testimonials-grid mt-4">
              <div
                v-for="(item, index) in additionalTestimonials"
                :key="index + 3"
                class="testimonial-card testimonial-fade-in"
              >
                <div class="testimonial-content">
                  <div class="testimonial-header">
                    <div class="testimonial-avatar">
                      <span>{{
                        getInitials(item.user?.name || "Khách hàng")
                      }}</span>
                    </div>
                    <div class="testimonial-info">
                      <h4 class="testimonial-name">
                        {{ item.name || "Khách hàng" }}
                      </h4>
                      <div class="testimonial-rating">
                        <i
                          v-for="n in 5"
                          :key="n"
                          class="fa-star fas"
                          :class="
                            n <= item.rate
                              ? 'text-warning'
                              : 'text-secondary opacity-25'
                          "
                        ></i>
                      </div>
                    </div>
                  </div>
                  <p class="testimonial-text">
                    "{{
                      item.content || "Dịch vụ tuyệt vời, tôi rất hài lòng!"
                    }}"
                  </p>
                  <!-- <div class="testimonial-footer">
                <span class="testimonial-age">{{ item.user?.tuoi || '...' }} tuổi</span>
                <div class="testimonial-decoration"></div>
              </div> -->
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Booking Section -->
    <section id="booking" class="booking">
      <div class="container">
        <div class="booking-content">
          <div class="booking-info">
            <h2>Đặt lịch hẹn</h2>
            <p>
              Hãy để chúng tôi chăm sóc bạn với những dịch vụ tốt nhất. Đặt lịch
              ngay hôm nay để nhận được ưu đãi đặc biệt và trải nghiệm không
              gian thư giãn tuyệt vời.
            </p>
            <ul class="booking-benefits">
              <li>Tư vấn miễn phí từ chuyên gia</li>
              <li>Đặt lịch linh hoạt theo thời gian bạn</li>
              <li>Giảm giá 10% cho khách hàng lần đầu</li>
              <li>Cam kết dịch vụ chất lượng cao</li>
              <li>Hỗ trợ 24/7 qua hotline</li>
            </ul>
          </div>

          <form class="booking-form" @submit.prevent="submitBooking">
            <div class="form-group">
              <label for="phone">Số điện thoại *</label>
              <input
                type="tel"
                id="phone"
                v-model="bookingForm.phone"
                placeholder="0123 456 789"
              />
            </div>

            <div class="form-group">
              <label for="service">Chọn dịch vụ *</label>
              <select
                id="service"
                v-model="selectedService"
                :disabled="bookingForm.consultAtStore"
              >
                <option value="">-- Chọn dịch vụ --</option>
                <option
                  v-for="service in services"
                  :key="service.id"
                  :value="service"
                >
                  {{ service.name }} - {{ service.price }} VNĐ
                </option>
              </select>
              <div class="form-row d-flex align-items-center gap-5">
                <button
                  type="button"
                  class="add-service-btn"
                  :disabled="!selectedService || bookingForm.consultAtStore"
                  @click="addService"
                >
                  Xác Nhận Dịch Vụ
                </button>
                <!-- Checkbox tư vấn tại quán -->
                <label class="consult-checkbox">
                  <input
                    type="checkbox"
                    v-model="bookingForm.consultAtStore"
                    style="width: 15px; height: 15px"
                    @change="handleConsultAtStore"
                  />
                  Tới quán nhân viên tư vấn
                </label>
              </div>
            </div>

            <!-- Selected Services List -->
            <div
              class="selected-services"
              v-if="bookingForm.services.length && !bookingForm.consultAtStore"
            >
              <h4 class="section-title">Dịch vụ đã chọn:</h4>
              <ul class="selected-services-list">
                <li
                  v-for="(service, index) in bookingForm.services"
                  :key="index"
                  class="selected-service-card"
                >
                  <!-- Thông tin dịch vụ -->
                  <div class="service-info">
                    <span class="service-name">{{ service.name }}</span>
                    <span class="service-price"
                      >{{ service.price.toLocaleString() }} VNĐ</span
                    >
                  </div>
                  <div class="service-actions">
                    <button
                      type="button"
                      class="remove-service-btn"
                      @click="removeService(index)"
                      title="Xóa dịch vụ"
                    >
                      &times;
                    </button>
                  </div>
                </li>
              </ul>
            </div>

            <div class="form-row">
              <div class="form-group">
                <label for="date">Ngày hẹn *</label>
                <input
                  type="date"
                  id="date"
                  v-model="bookingForm.date"
                  :min="minDate"
                  required
                />
              </div>

              <div class="form-group">
                <label for="time">Giờ hẹn *</label>
                <select id="time" v-model="bookingForm.time">
                  <option value="">-- Chọn giờ --</option>
                  <option
                    v-for="slot in availableSlots"
                    :key="slot.khungGio"
                    :value="slot.khungGio"
                  >
                    {{ slot.khungGio }}
                  </option>
                </select>
              </div>
            </div>

            <div class="form-group">
              <label for="notes">Ghi chú</label>
              <textarea
                id="notes"
                v-model="bookingForm.notes"
                rows="4"
                placeholder="Những yêu cầu đặc biệt hoặc ghi chú khác..."
              ></textarea>
            </div>

            <button type="submit" class="submit-btn">Đặt lịch hẹn</button>
          </form>
        </div>
      </div>
    </section>

    <!-- Improved About Section -->
    <section id="about" class="about-section">
      <div class="container">
        <div class="row align-items-center">
          <!-- Cột trái: Nội dung -->
          <div class="col-lg-6">
            <div class="about-content-wrapper">
              <h2 class="about-title">
                <span class="title-accent">Về</span> TutaSpa
              </h2>
              <div class="about-subtitle">
                Điểm đến hoàn hảo cho sự thư giãn và làm đẹp
              </div>
              <p class="about-description">
                Với hơn <strong>10 năm kinh nghiệm</strong> trong ngành làm đẹp,
                TutaSpa tự hào mang đến cho khách hàng những trải nghiệm thư
                giãn tuyệt vời nhất. Chúng tôi kết hợp tinh hoa truyền thống với
                công nghệ hiện đại, sử dụng 100% sản phẩm từ thiên nhiên.
              </p>

              <div class="about-features">
                <div class="feature-item">
                  <div class="feature-icon-wrapper">
                    <span class="feature-emoji">🌿</span>
                  </div>
                  <div class="feature-content">
                    <h4>Sản phẩm hữu cơ cao cấp</h4>
                    <p>Nhập khẩu trực tiếp từ các thương hiệu uy tín</p>
                  </div>
                </div>

                <div class="feature-item">
                  <div class="feature-icon-wrapper">
                    <span class="feature-emoji">💆‍♀️</span>
                  </div>
                  <div class="feature-content">
                    <h4>Đội ngũ chuyên gia quốc tế</h4>
                    <p>Được đào tạo bài bản tại Hàn Quốc và Nhật Bản</p>
                  </div>
                </div>

                <div class="feature-item">
                  <div class="feature-icon-wrapper">
                    <span class="feature-emoji">🏆</span>
                  </div>
                  <div class="feature-content">
                    <h4>Chứng nhận ISO 9001:2015</h4>
                    <p>Đảm bảo chất lượng dịch vụ quốc tế</p>
                  </div>
                </div>

                <div class="feature-item">
                  <div class="feature-icon-wrapper">
                    <span class="feature-emoji">💯</span>
                  </div>
                  <div class="feature-content">
                    <h4>Cam kết hài lòng 100%</h4>
                    <p>Hoàn tiền nếu không hài lòng với dịch vụ</p>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Cột phải: Hình ảnh -->
          <div class="col-lg-6">
            <div class="about-visual">
              <div class="about-image-main">
                <img
                  src="https://images.unsplash.com/photo-1630595271375-5073a6c0638b?w=600&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MzZ8fHNwYXxlbnwwfHwwfHx8MA%3D%3D"
                  alt="TutaSpa Interior"
                  class="img-fluid rounded-4 shadow-lg"
                />
                <div class="image-overlay">
                  <div class="overlay-content">
                    <h3>Không gian thư giãn</h3>
                    <p>Thiết kế sang trọng, yên tĩnh</p>
                  </div>
                </div>
              </div>

              <!-- <div class="about-stats">
            <div class="stat-item">
              <div class="stat-number">5000+</div>
              <div class="stat-label">Khách hàng tin tưởng</div>
            </div>
            <div class="stat-item">
              <div class="stat-number">50+</div>
            <div class="stat-label">Dịch vụ chuyên nghiệp</div>
            </div>
            <div class="stat-item">
              <div class="stat-number">98%</div>
              <div class="stat-label">Tỷ lệ hài lòng</div>
            </div>
          </div> -->

              <!-- Google Map -->
              <div class="col-lg-12">
                <div class="map-wrapper rounded-4 overflow-hidden shadow h-100">
                  <a
                    href="https://www.google.com/maps/place/31+Nguyễn+Mộng+Tuân,+Hoà+Minh,+Liên+Chiểu,+Đà+Nẵng+550000"
                    target="_blank"
                    class="d-block w-100 h-100"
                  >
                    <iframe
                      src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3834.168381775547!2d108.1698262!3d16.0633039!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x3142191ed695fc89%3A0xb837389a53933ca9!2zMzEgTmfDtG4gTeG7mW5nIFR1w6puLCBIb8OgIE1pbmgsIExpw6puIENow61hLCDEkMOgIE5hbmc!5e0!3m2!1svi!2s!4v1721903400000"
                      width="100%"
                      height="100%"
                      style="border: 0"
                      allowfullscreen=""
                      loading="lazy"
                      referrerpolicy="no-referrer-when-downgrade"
                      title="31 Nguyễn Mộng Tuân, Đà Nẵng"
                    ></iframe>
                  </a>
                </div>
                <a
                  href="https://www.google.com/maps/place/31+Nguyễn+Mộng+Tuân,+Hoà+Minh,+Liên+Chiểu,+Đà+Nẵng+550000"
                  target="_blank"
                  class="text-decoration-none text-dark mt-2 d-block"
                >
                  <i
                    class="fa-solid fa-map-location-dot me-2 text-primary-green"
                  ></i
                  >Xem bản đồ
                </a>
              </div>

              <div class="floating-badge">
                <div class="badge-content">
                  <span class="badge-icon">⭐</span>
                  <div class="badge-text">
                    <div class="badge-title">Top Rated</div>
                    <div class="badge-subtitle">Spa in Da Nang</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, onMounted, watch, computed } from "vue";
import apiClient from "../utils/axiosClient";
import Swal from "sweetalert2";
// Reactive state
const isLoading = ref(false);
const services = ref([]);
const categories = ref([]);
const currentCategory = ref("all");
const filteredServices = ref([]);
const loading = ref(true);
const error = ref(null);
const minDate = ref("");
const selectedService = ref(null);
const bookingForm = ref({
  phone: "",
  services: [],
  date: new Date().toISOString().split("T")[0],
  time: "",
  notes: "",
  consultAtStore: false,
});
const modalForm = ref({
  name: "",
  phone: "",
  email: "",
  date: "",
  time: "",
  notes: "",
});

const showAllTestimonials = ref(false);

const availableSlots = ref([]);
// Base URL for images
const IMAGE_BASE_URL =
  import.meta.env.VITE_BASE_URL.replace("/api", "") + "/images/";

const currentSlide = ref(0);

const slides = [
  {
    image:
      "https://images.unsplash.com/photo-1540555700478-4be289fbecef?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80",
    title: "Thư Giãn & Tái Tạo",
    subtitle: "Trải nghiệm không gian yên bình",
  },
  {
    image:
      "https://plus.unsplash.com/premium_photo-1723867490491-10519f8ed969?w=600&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MjF8fHNwYXxlbnwwfHwwfHx8MA%3D%3D",
    title: " Chăm Sóc Chuyên Nghiệp",
    subtitle: "Với đội ngũ chuyên gia giàu kinh nghiệm",
  },
  {
    image:
      "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80",
    title: "Làm Đẹp Tự Nhiên",
    subtitle: "Sử dụng 100% sản phẩm thiên nhiên",
  },
];

const stats = [
  { number: "10+", label: "Năm kinh nghiệm", icon: "🏆" },
  { number: "5000+", label: "Khách hàng hài lòng", icon: "👥" },
  { number: "24", label: "Dịch vụ chuyên nghiệp", icon: "🌿" },
];

const testimonials = ref([]);

// Computed testimonials
const visibleTestimonials = computed(() => testimonials.value.slice(0, 3));
const additionalTestimonials = computed(() => testimonials.value.slice(3));

const getInitials = (name) =>
  name
    .split(" ")
    .map((word) => word.charAt(0))
    .join("")
    .toUpperCase()
    .substring(0, 2);

const toggleShowAll = () => {
  showAllTestimonials.value = !showAllTestimonials.value;
};

// Methods
const handleConsultAtStore = () => {
  if (bookingForm.value.consultAtStore) {
    bookingForm.value.services = [];
    selectedService.value = null;
  }
};

const addServiceFromCard = (service) => {
  if (
    !bookingForm.value.services.some((s) => s.id === service.id) &&
    !bookingForm.value.consultAtStore
  ) {
    bookingForm.value.services.push({ ...service, soLuong: 1 });
  }
  const bookingSection = document.getElementById("booking");
  if (bookingSection) {
    bookingSection.scrollIntoView({ behavior: "smooth" });
  }
};

const fetchSlots = async () => {
  if (!bookingForm.value.date) return;
  try {
    const res = await apiClient.get("/DatLich/slots", {
      params: { ngay: bookingForm.value.date },
    });
    availableSlots.value = res.filter((slot) => slot.conLai > 0);
  } catch (err) {
    console.error("Lỗi khi lấy giờ hẹn:", err);
    availableSlots.value = [];
  }
};

// Watch đổi ngày để lấy slot
watch(
  () => bookingForm.value.date,
  () => {
    fetchSlots();
    bookingForm.value.time = "";
  }
);

const fetchCategories = async () => {
  try {
    const response = await apiClient.get("/LoaiDichVu");
    const data = response;
    categories.value = Array.isArray(data) ? data : data.data || [];
    if (categories.value.length === 0)
      throw new Error("Không có danh mục dịch vụ nào");
  } catch (err) {
    console.error("Lỗi khi tải danh mục:", err);
    error.value = "Không thể tải danh mục dịch vụ. Hiển thị mặc định.";
    categories.value = [
      { loaiDichVuID: 1, tenLoai: "Triệt lông" },
      { loaiDichVuID: 2, tenLoai: "Massage" },
      { loaiDichVuID: 3, tenLoai: "Chăm sóc da" },
    ];
  }
};

const fetchServices = async () => {
  try {
    const response = await apiClient.get("/DichVu");
    const data = response;
    const serviceData = Array.isArray(data) ? data : data.data || [];
    services.value = serviceData
      .filter((service) => service.trangThai === 1)
      .map((service) => ({
        id: service.dichVuID,
        name: service.tenDichVu,
        category:
          categories.value.find(
            (cat) => cat.loaiDichVuID === service.loaiDichVuID
          )?.tenLoai || "Khác",
        price: service.gia.toLocaleString("vi-VN"),
        duration: service.thoiGian,
        description: service.moTa,
        image: `${IMAGE_BASE_URL}${service.hinhAnh}`,
        mucDanhGia: service.mucDanhGia || 0,
      }));
    if (services.value.length === 0) throw new Error("Không có dịch vụ nào");
  } catch (err) {
    console.error("Lỗi khi tải dịch vụ:", err);
    error.value = "Không thể tải dịch vụ. Hiển thị mặc định.";
  }
};

const filterServices = (category) => {
  currentCategory.value = category;
  filteredServices.value =
    category === "all"
      ? [...services.value]
      : services.value.filter((service) => service.category === category);
};

const addService = () => {
  if (
    selectedService.value &&
    !bookingForm.value.services.some(
      (s) => s.id === selectedService.value.id
    ) &&
    !bookingForm.value.consultAtStore
  ) {
    bookingForm.value.services.push({ ...selectedService.value, soLuong: 1 });
    selectedService.value = null; // Reset dropdown
  }
};

const removeService = (index) => {
  bookingForm.value.services.splice(index, 1);
};

const submitBooking = async () => {
  try {
    if (!bookingForm.value.phone) {
      await Swal.fire({
        icon: "warning",
        title: "Thiếu thông tin",
        text: "Vui lòng nhập số điện thoại!",
        confirmButtonText: "OK",
      });
      return;
    }
    if (
      !bookingForm.value.services.length &&
      !bookingForm.value.consultAtStore
    ) {
      await Swal.fire({
        icon: "warning",
        title: "Thiếu thông tin",
        text: "Vui lòng chọn ít nhất một dịch vụ hoặc chọn tư vấn tại quán!",
        confirmButtonText: "OK",
      });
      return;
    }
    if (!bookingForm.value.time) {
      await Swal.fire({
        icon: "warning",
        title: "Thiếu thông tin",
        text: "Vui lòng chọn thời gian!",
        confirmButtonText: "OK",
      });
      return;
    }
    isLoading.value = true;
    Swal.fire({
      title: "Đang xử lý...",
      text: "Vui lòng đợi trong giây lát",
      allowOutsideClick: false,
      didOpen: () => {
        Swal.showLoading();
      },
    });
    const thoiGian = new Date(
      new Date(
        `${bookingForm.value.date}T${bookingForm.value.time}`
      ).getTime() +
        7 * 60 * 60 * 1000
    ).toISOString();

    const dichVus = bookingForm.value.consultAtStore
      ? []
      : bookingForm.value.services.map((s) => ({
          dichVuID: s.id,
          soLuong: 1,
        }));

    const payload = {
      soDienThoai: bookingForm.value.phone,
      thoiGian,
      dichVus,
      ghiChu: bookingForm.value.notes,
      datTruoc: true,
    };

    const res = await apiClient.post("/DatLich", payload);

    await Swal.fire({
      icon: "success",
      title: "Thành công",
      text: "Đặt lịch thành công!",
      confirmButtonText: "OK",
    });
    resetBookingForm();
  } catch (err) {
    console.error("Lỗi đặt lịch:", err);
    alert("Đặt lịch thất bại!");
  }finally {
    isLoading.value = false;
  }
};

const resetBookingForm = () => {
  bookingForm.value = {
    phone: "",
    services: [],
    date: new Date().toISOString().split("T")[0],
    time: "",
    notes: "",
    consultAtStore: false,
  };
  selectedService.value = null;
};

// Lifecycle hook

const resetModalForm = () => {
  modalForm.value = {
    name: "",
    phone: "",
    email: "",
    date: "",
    time: "",
    notes: "",
  };
};

// ✅ Gom tất cả vào một onMounted duy nhất
onMounted(async () => {
  try {
    minDate.value = new Date().toISOString().split("T")[0];
    loading.value = true;

    // Load dữ liệu
    await fetchCategories();
    await fetchServices();
    filterServices("all");
    await fetchSlots();

    // Load testimonials
    const res = await apiClient.get("/DanhGia/admin/highlights");
    testimonials.value = res;
  } catch (err) {
    console.error("Lỗi khi khởi tạo:", err);
  } finally {
    loading.value = false;
  }
});
</script>

<style>
.carousel-item {
  height: 100vh;
  background-size: cover !important;
  background-position: center !important;
  position: relative;
}

.carousel-caption {
  position: absolute;
  top: 0; /* chiếm toàn bộ */
  left: 0;
  right: 0;
  bottom: 0;

  text-align: center;
  padding: 0 15px;
}

.font-lora {
  font-family: "Lora", serif;
}

/* Stats container improvements - FIXED */
.d-flex.flex-wrap.justify-content-center.gap-4.mb-5 {
  gap: 2rem !important;
  margin-bottom: 2rem !important;
  padding: 0 1rem;
}

.d-flex.flex-wrap.justify-content-center.gap-4.mb-5 > div {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 1.5rem;
  min-width: 150px;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.stat-number {
  font-size: 2.5rem;
  font-weight: bold;
  color: #fbbf24;
}

.stat-label {
  font-size: 0.9rem;
  opacity: 0.9;
}

/* Features Section */
.features {
  padding: 6rem 2rem;
  background: white;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(4, minmax(280px, 1fr));
  gap: 3rem;
  max-width: 1200px;
  margin: 0 auto;
}

.feature-card {
  text-align: center;
  padding: 2rem;
  border-radius: 15px;
  background: linear-gradient(135deg, #f0fdf4 0%, #dcfce7 100%);
  transition: all 0.3s ease;
}

.feature-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 25px rgba(120, 186, 126, 0.2);
}

.feature-icon {
  font-size: 3rem;
  margin-bottom: 1rem;
  color: #78ba7e;
}

.feature-card h3 {
  font-size: 1.4rem;
  margin-bottom: 1rem;
  color: #2d4a2d;
}

/* Services Section */
.services {
  padding: 8rem 2rem;
  background: linear-gradient(135deg, #f8fdf8 0%, #f0fdf4 50%, #e8f5e8 100%);
}

.section-title {
  text-align: center;
  font-size: 2.8rem;
  margin-bottom: 1rem;
  color: #335333;
  position: relative;
  font-family: "Lora", serif;
}

.section-subtitle {
  text-align: center;
  font-size: 1.1rem;
  color: #4b5563;
  margin-bottom: 4rem;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}

.section-title::after {
  content: "";
  display: block;
  width: 80px;
  height: 3px;
  background: linear-gradient(45deg, #78ba7e, #5e8c64);
  margin: 1rem auto 0;
  border-radius: 2px;
}

.service-categories {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 3rem;
  flex-wrap: wrap;
}

.category-btn {
  background: white;
  border: 2px solid #78ba7e;
  color: #78ba7e;
  padding: 0.8rem 2rem;
  border-radius: 25px;
  cursor: pointer;
  font-weight: 500;
  transition: all 0.3s ease;
}

.category-btn.active,
.category-btn:hover {
  background: #78ba7e;
  color: white;
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(120, 186, 126, 0.4);
}

.services-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 2.5rem;
  margin-top: 3rem;
}

.service-card {
  background: white;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 8px 25px rgba(120, 186, 126, 0.12);
  transition: all 0.4s ease;
  position: relative;
  border: 1px solid #e8f5e8;
  height: 100%;
  display: flex;
  flex-direction: column;
}

.service-card::before {
  content: "";
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(120, 186, 126, 0.15),
    transparent
  );
  transition: all 0.6s;
  z-index: 1;
}

.service-card:hover::before {
  left: 100%;
}

.service-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 20px 40px rgba(120, 186, 126, 0.2);
}

.service-image-container {
  position: relative;
  overflow: hidden;
}

.service-image {
  width: 100%;
  height: 220px;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.service-card:hover .service-image {
  transform: scale(1.05);
}

.service-rating-overlay {
  position: absolute;
  top: 1rem;
  right: 1rem;
  background: rgba(255, 255, 255, 0.95);
  padding: 0.5rem 0.75rem;
  border-radius: 20px;
  backdrop-filter: blur(10px);
}

.service-rating {
  display: flex;
  align-items: center;
  gap: 0.25rem;
  font-size: 0.9rem;
}

.rating-star {
  color: #d1d5db;
  font-size: 1rem;
  transition: color 0.2s;
}

.rating-star.filled {
  color: #fbbf24;
}

.rating-star.half-filled {
  background: linear-gradient(90deg, #fbbf24 50%, #d1d5db 50%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.rating-text {
  font-size: 0.8rem;
  color: #6b7280;
  font-weight: 600;
}

.service-content {
  padding: 2rem;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.service-title {
  font-size: 1.4rem;
  margin-bottom: 1rem;
  color: #2d4a2d;
  font-family: "Lora", serif;
  font-weight: 600;
}

.service-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  gap: 1rem;
}

.service-duration {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background: #f0fdf4;
  color: #4a6741;
  padding: 0.4rem 1rem;
  border-radius: 15px;
  font-size: 0.85rem;
  font-weight: 500;
}

.duration-icon {
  font-size: 1rem;
}

.service-price {
  gap: 1.3rem;
  font-weight: bold;
  color: #f59e0b;
  font-family: "Lora", serif;
}

.service-description {
  color: #6b7280;
  margin-bottom: auto;
  line-height: 1.6;
  flex: 1;
}

.service-book-btn {
  background: linear-gradient(45deg, #78ba7e, #5e8c64);
  color: white;
  padding: 0.9rem 2rem;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  font-weight: 600;
  transition: all 0.3s ease;
  font-size: 0.95rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  margin-top: 1rem;
  bottom: 10px;
}

.service-book-btn:hover {
  transform: scale(1.05);
  box-shadow: 0 6px 20px rgba(120, 186, 126, 0.5);
}

.btn-arrow {
  font-size: 1.2rem;
  transition: transform 0.3s ease;
}

.service-book-btn:hover .btn-arrow {
  transform: translateX(3px);
}

/* Improved Testimonials Section */
.testimonials-section {
  padding: 8rem 2rem;
  background: linear-gradient(135deg, #cef8d2 20%, #6ba371 50%, #95dda0 100%);
  position: relative;
  overflow: hidden;
}

.testimonials-section::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><defs><pattern id="grain" width="100" height="100" patternUnits="userSpaceOnUse"><circle cx="25" cy="25" r="1" fill="white" opacity="0.05"/><circle cx="75" cy="75" r="1" fill="white" opacity="0.05"/><circle cx="50" cy="10" r="0.5" fill="white" opacity="0.03"/><circle cx="10" cy="60" r="0.5" fill="white" opacity="0.03"/></pattern></defs><rect width="100" height="100" fill="url(%23grain)"/></svg>');
  pointer-events: none;
}

.testimonials-section .section-title {
  color: white;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

.testimonials-section .section-title::after {
  background: linear-gradient(45deg, #fbbf24, #f59e0b);
}

.testimonials-container {
  position: relative;
  z-index: 2;
}

.testimonials-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 2rem;
  margin-bottom: 2rem;
}

.testimonial-card {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  padding: 2rem;
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
  transition: all 0.4s ease;
  border: 1px solid rgba(255, 255, 255, 0.2);
  position: relative;
  overflow: hidden;
}

.testimonial-card::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, #78ba7e, #fbbf24, #78ba7e);
  transform: scaleX(0);
  transition: transform 0.3s ease;
}

.testimonial-card:hover::before {
  transform: scaleX(1);
}

.testimonial-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.15);
  background: rgba(255, 255, 255, 1);
}

.testimonial-featured {
  grid-column: span 1;
  background: rgba(255, 255, 255, 1);
  border: 2px solid rgb(255, 255, 255);
}

.testimonial-content {
  position: relative;
  z-index: 2;
}

.testimonial-header {
  display: flex;
  align-items: center;
  margin-bottom: 1.5rem;
}

.testimonial-avatar {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: linear-gradient(135deg, #78ba7e, #5e8c64);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 1rem;
  flex-shrink: 0;
}

.testimonial-avatar span {
  color: white;
  font-weight: bold;
  font-size: 1.2rem;
}

.testimonial-info {
  flex: 1;
}

.testimonial-name {
  font-size: 1.1rem;
  font-weight: 600;
  color: #2d4a2d;
  margin: 0 0 0.5rem 0;
}

.testimonial-rating {
  display: flex;
  gap: 2px;
}

.testimonial-rating i {
  font-size: 1rem;
}

.testimonial-text {
  color: #4b5563;
  font-style: italic;
  line-height: 1.7;
  margin-bottom: 1.5rem;
  position: relative;
  padding-left: 1rem;
}

.testimonial-text::before {
  content: '"';
  position: absolute;
  left: -0.2rem;
  top: -0.5rem;
  font-size: 3rem;
  color: #78ba7e;
  opacity: 0.3;
  line-height: 1;
}

.testimonial-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.testimonial-age {
  color: #6b7280;
  font-size: 0.9rem;
}

.testimonial-decoration {
  width: 30px;
  height: 2px;
  background: linear-gradient(90deg, #78ba7e, #fbbf24);
  border-radius: 1px;
}

.show-more-btn {
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(10px);
  color: white;
  border: 2px solid rgba(255, 255, 255, 0.3);
  padding: 1rem 2rem;
  border-radius: 50px;
  cursor: pointer;
  font-weight: 600;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.show-more-btn:hover {
  background: rgba(255, 255, 255, 0.3);
  border-color: rgba(255, 255, 255, 0.5);
  transform: translateY(-2px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
}

.no-reviews-icon {
  font-size: 4rem;
  opacity: 0.5;
  margin-bottom: 1rem;
}

.additional-testimonials {
  animation: fadeInUp 0.5s ease;
}

.testimonial-fade-in {
  animation: fadeInUp 0.5s ease;
}

/* Improved About Section */
.about-section {
  padding: 6rem 0rem;
  background: linear-gradient(135deg, #f8fdf8 0%, #f0fdf4 50%, #e8f5e8 100%);
  position: relative;
  overflow: hidden;
}

.about-section::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><defs><pattern id="dots" width="20" height="20" patternUnits="userSpaceOnUse"><circle cx="10" cy="10" r="1" fill="%2378ba7e" opacity="0.1"/></pattern></defs><rect width="100" height="100" fill="url(%23dots)"/></svg>');
  pointer-events: none;
}

.about-content-wrapper {
  position: relative;
  z-index: 2;
}

.about-title {
  font-size: 3rem;
  font-weight: 700;
  color: #2d4a2d;
  margin-bottom: 1rem;
  font-family: "Lora", serif;
}

.title-accent {
  background: linear-gradient(45deg, #78ba7e, #5e8c64);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.about-subtitle {
  font-size: 1.3rem;
  color: #6b7280;
  margin-bottom: 2rem;
  font-style: italic;
}

.about-description {
  font-size: 1.1rem;
  line-height: 1.8;
  color: #374151;
  margin-bottom: 3rem;
}

.about-features {
  display: grid;
  gap: 2rem;
  margin-bottom: 3rem;
}

.feature-item {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  padding: 1.5rem;
  background: rgba(255, 255, 255, 0.7);
  border-radius: 15px;
  transition: all 0.3s ease;
  border: 1px solid rgba(120, 186, 126, 0.1);
}

.feature-item:hover {
  transform: translateX(10px);
  box-shadow: 0 10px 25px rgba(120, 186, 126, 0.15);
  background: rgba(255, 255, 255, 0.9);
}

.feature-icon-wrapper {
  width: 60px;
  height: 60px;
  border-radius: 15px;
  background: linear-gradient(135deg, #78ba7e, #5e8c64);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.feature-emoji {
  font-size: 1.8rem;
}

.feature-content h4 {
  font-size: 1.2rem;
  font-weight: 600;
  color: #2d4a2d;
  margin-bottom: 0.5rem;
}

.feature-content p {
  color: #6b7280;
  margin: 0;
  line-height: 1.5;
}

.about-stats {
  display: flex;
  justify-content: space-between;
  padding: 2rem;
  background: linear-gradient(135deg, #78ba7e, #5e8c64);
  border-radius: 20px;
  color: white;
  box-shadow: 0 15px 35px rgba(120, 186, 126, 0.3);
}
/* map */
.map-wrapper iframe {
  width: 100%;
  height: 430px;
  border: 0;
}

.stat-item {
  text-align: center;
  flex: 1;
}

.stat-number {
  font-size: 2.5rem;
  font-weight: 700;
  color: #fbbf24;
  display: block;
  margin-bottom: 0.5rem;
}

.stat-label {
  font-size: 0.9rem;
  opacity: 0.9;
}

.about-visual {
  position: relative;
  z-index: 2;
}

.about-image-main {
  position: relative;
  margin-bottom: 2rem;
}

.about-image-main img {
  width: 100%;
  height: 400px;
  object-fit: cover;
}

.image-overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: linear-gradient(transparent, rgba(0, 0, 0, 0.8));
  color: white;
  padding: 2rem;
  border-radius: 0 0 1rem 1rem;
}

.overlay-content h3 {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
}

.overlay-content p {
  margin: 0;
  opacity: 0.9;
}

.about-image-secondary {
  position: relative;
  max-width: 200px;
  margin-left: auto;
}

.about-image-secondary img {
  width: 100%;
  height: 150px;
  object-fit: cover;
}

.floating-badge {
  position: absolute;
  top: 2rem;
  right: 2rem;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  padding: 1rem;
  border-radius: 15px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.badge-content {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.badge-icon {
  font-size: 1.5rem;
}

.badge-title {
  font-weight: 600;
  color: #2d4a2d;
  font-size: 0.9rem;
}

.badge-subtitle {
  color: #6b7280;
  font-size: 0.8rem;
}

/* Booking Section */
.booking {
  padding: 8rem 2rem;
  background: linear-gradient(135deg, #a6ecab 0%, #6ba371 50%, #5e8c64 100%);
  color: white;
}

.booking-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  max-width: 1200px;
  margin: 0 auto;
  align-items: center;
}

.booking-info h2 {
  font-size: 2.5rem;
  margin-bottom: 2rem;
  font-family: "Lora", serif;
}

.booking-info p {
  font-size: 1.1rem;
  margin-bottom: 2rem;
  opacity: 0.9;
}

.booking-benefits {
  list-style: none;
  margin-bottom: 2rem;
}

.booking-benefits li {
  margin-bottom: 1rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.booking-benefits li::before {
  content: "✓";
  color: #fbbf24;
  font-weight: bold;
  font-size: 1.2rem;
}

.booking-form {
  background: rgba(255, 255, 255, 0.15);
  padding: 3rem;
  border-radius: 20px;
  backdrop-filter: blur(10px);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 2rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 600;
  color: white;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 1rem;
  border: none;
  border-radius: 10px;
  font-size: 1rem;
  background: rgba(255, 255, 255, 0.9);
  transition: all 0.3s ease;
  color: #2d4a2d;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  outline: none;
  background: white;
  transform: scale(1.02);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.submit-btn {
  background: linear-gradient(45deg, #f59e0b, #f97316);
  color: white;
  padding: 1.2rem 3rem;
  border: none;
  border-radius: 50px;
  font-size: 1.1rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
  width: 100%;
  margin-top: 1rem;
}

.submit-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 25px rgba(245, 158, 11, 0.4);
}

.add-service-btn {
  background: linear-gradient(45deg, #308238, #5e8c64);
  color: white;
  padding: 0.8rem 2rem;
  border: none;
  border-radius: 25px;
  margin-top: 1rem;
  cursor: pointer;
}

.section-title {
  font-size: 2.3rem;
  font-weight: 800;
  color: #1f5932;
  margin-bottom: 1rem;
}

.selected-services-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.selected-service-card {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #f6f9f7;
  border-radius: 12px;
  padding: 1rem 1.5rem;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
  transition: transform 0.2s ease;
}

.selected-service-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.1);
}

.service-info {
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}

.service-name {
  font-weight: 600;
  font-size: 1rem;
  color: #333;
}

.service-price {
  color: #ffaa16;
  font-size: 0.95rem;
}

.service-actions {
  display: flex;
  align-items: center;
  gap: 0.6rem;
}

.quantity-label {
  font-size: 0.85rem;
  color: #555;
}

.quantity-input {
  width: 60px;
  padding: 0.3rem;
  border: 1px solid #ddd;
  border-radius: 6px;
  text-align: center;
}

.remove-service-btn {
  background: #ff4d4d;
  border: none;
  color: white;
  font-size: 1.2rem;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  cursor: pointer;
  transition: background 0.3s ease;
}

.remove-service-btn:hover {
  background: #e60000;
}

/* Loading and Error States */
.loading {
  text-align: center;
  padding: 4rem 0;
  color: #78ba7e;
}

.loading-spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #e8f5e8;
  border-top: 4px solid #78ba7e;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 1rem;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.error {
  text-align: center;
  padding: 4rem 0;
  color: #dc2626;
  background: #fef2f2;
  border-radius: 10px;
  margin: 2rem 0;
}

/* Animations */
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* RESPONSIVE STYLES - FIXED CAROUSEL */

/* Desktop Large */
@media (max-width: 1200px) {
  .container {
    max-width: 100%;
  }

  .carousel-caption {
    width: 95%;
  }

  .display-2 {
    font-size: 2.5rem !important;
  }

  .fs-3 {
    font-size: 1.4rem !important;
  }

  .lead {
    font-size: 1rem !important;
  }

  .services-grid {
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  }
}

/* Desktop Medium */
@media (max-width: 992px) {
  .carousel-item {
    height: 90vh;
  }

  .display-2 {
    font-size: 2.2rem !important;
    line-height: 1.2;
  }

  .fs-3 {
    font-size: 1.2rem !important;
  }

  .lead {
    font-size: 0.95rem !important;
    max-width: 600px !important;
  }

  .d-flex.flex-wrap.justify-content-center.gap-4.mb-5 {
    gap: 1.5rem !important;
    flex-wrap: wrap;
  }

  .d-flex.flex-wrap.justify-content-center.gap-4.mb-5 > div {
    min-width: 120px;
    padding: 1rem;
  }

  .stat-number {
    font-size: 2rem;
  }

  .hero-title {
    font-size: 2.8rem;
  }

  .hero-subtitle {
    font-size: 1.3rem;
  }

  .stats-container {
    gap: 1.5rem;
  }

  .booking-content {
    grid-template-columns: 1fr;
    gap: 3rem;
  }

  .section-title {
    font-size: 2.3rem;
  }
}

/* Tablet */
@media (max-width: 768px) {
  .carousel-item {
    height: 85vh;
  }

  .carousel-caption {
    width: 95%;
    padding: 0 1rem;
  }

  .display-2 {
    font-size: 1.8rem !important;
    margin-bottom: 1rem !important;
  }

  .fs-3 {
    font-size: 1rem !important;
    margin-bottom: 1rem !important;
  }

  .lead {
    font-size: 0.9rem !important;
    margin-bottom: 1.5rem !important;
    line-height: 1.5;
  }

  /* Stats layout for tablet - FIXED */
  .d-flex.flex-wrap.justify-content-center.gap-4.mb-5 {
    display: grid !important;
    grid-template-columns: repeat(3, 1fr);
    gap: 1rem !important;
    margin-bottom: 1.5rem !important;
    padding: 0 0.5rem;
  }

  .d-flex.flex-wrap.justify-content-center.gap-4.mb-5 > div {
    min-width: auto;
    padding: 0.8rem 0.5rem;
    text-align: center;
  }

  .stat-number {
    font-size: 1.5rem;
    margin-bottom: 0.2rem;
  }

  .stat-label {
    font-size: 0.75rem;
    line-height: 1.2;
  }

  /* Button adjustments */
  .d-flex.flex-column.flex-sm-row.justify-content-center.gap-3 {
    flex-direction: column !important;
    gap: 1rem !important;
    padding: 0 1rem;
  }

  .btn-lg {
    padding: 0.8rem 2rem !important;
    font-size: 0.9rem !important;
  }

  .features {
    padding: 4rem 1rem;
  }

  .services {
    padding: 6rem 1rem;
  }

  .booking {
    padding: 6rem 1rem;
  }

  .features-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }

  .services-grid {
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 2rem;
  }

  .form-row {
    grid-template-columns: 1fr;
  }

  .category-btn {
    font-size: 0.9rem;
    padding: 0.6rem 1.5rem;
  }

  .section-title {
    font-size: 2rem;
  }

  .section-subtitle {
    font-size: 1rem;
  }

  .service-image {
    height: 180px;
  }

  .service-content {
    padding: 1.5rem;
  }

  .service-title {
    font-size: 1.2rem;
  }

  .service-meta {
    flex-direction: column;
    align-items: flex-start;
    gap: 0.5rem;
  }

  .service-duration {
    font-size: 0.8rem;
  }

  .service-price {
    font-size: 1.2rem;
  }

  .hero-title {
    font-size: 2.2rem;
    line-height: 1.1;
  }

  .hero-subtitle {
    font-size: 1.1rem;
  }

  .hero-description {
    font-size: 1rem;
    padding: 0 1rem;
  }

  .stats-container {
    gap: 1rem;
  }

  .stat-item {
    min-width: 100px;
  }

  .stat-icon {
    font-size: 2rem;
  }

  .stat-number {
    font-size: 1.5rem;
  }

  .stat-label {
    font-size: 0.8rem;
  }

  .hero-buttons {
    padding: 0 1rem;
  }

  .hero-buttons .btn {
    font-size: 0.95rem;
    padding: 0.8rem 2rem;
  }

  .booking-form {
    padding: 2rem;
  }

  .booking-info h2 {
    font-size: 2rem;
  }

  .form-actions {
    flex-direction: column;
    align-items: stretch;
  }

  .consult-checkbox {
    justify-content: center;
  }

  .selected-service-item {
    flex-direction: column;
    gap: 0.5rem;
    align-items: stretch;
  }

  .quantity-control {
    justify-content: space-between;
  }

  .testimonials-grid {
    flex-direction: column;
  }

  .testimonial-card {
    min-width: auto;
  }

  /* Carousel controls adjustments */
  .carousel-control-prev,
  .carousel-control-next {
    width: 8%;
  }

  .carousel-control-prev-icon,
  .carousel-control-next-icon {
    width: 20px;
    height: 20px;
  }
}

/* Mobile Large */
@media (max-width: 576px) {
  .carousel-item {
    height: 80vh;
    min-height: 500px;
  }

  .carousel-caption {
    width: 100%;
    padding: 0 0.5rem;
    top: 45%;
  }

  .display-2 {
    font-size: 1.4rem !important;
    margin-bottom: 0.5rem !important;
    line-height: 1.3;
  }

  .fs-3 {
    font-size: 0.9rem !important;
    margin-bottom: 0.8rem !important;
  }

  .lead {
    font-size: 0.8rem !important;
    margin-bottom: 1rem !important;
    line-height: 1.4;
    padding: 0 !important;
  }

  /* Stats - vertical stack on mobile - FIXED */
  .d-flex.flex-wrap.justify-content-center.gap-4.mb-5 {
    display: flex !important;
    flex-direction: column;
    align-items: center;
    gap: 0.8rem !important;
    margin-bottom: 1rem !important;
    padding: 0;
  }

  .d-flex.flex-wrap.justify-content-center.gap-4.mb-5 > div {
    display: flex;
    align-items: center;
    gap: 0.8rem;
    width: 100%;
    max-width: 280px;
    padding: 0.6rem 1rem;
    margin: 0;
    justify-content: center;
  }

  .d-flex.flex-wrap.justify-content-center.gap-4.mb-5 > div .fs-1 {
    font-size: 1.2rem !important;
    margin: 0 !important;
  }

  .d-flex.flex-wrap.justify-content-center.gap-4.mb-5 > div .fs-3 {
    font-size: 1rem !important;
    margin: 0 0.3rem 0 0 !important;
  }

  .d-flex.flex-wrap.justify-content-center.gap-4.mb-5 > div .text-white {
    font-size: 0.7rem !important;
    margin: 0 !important;
    line-height: 1.2;
  }

  /* Buttons */
  .d-flex.flex-column.flex-sm-row.justify-content-center.gap-3 {
    padding: 0 0.5rem;
    gap: 0.8rem !important;
  }

  .btn-lg {
    padding: 0.7rem 1.5rem !important;
    font-size: 0.8rem !important;
    border-radius: 20px !important;
  }

  /* Hero Section */
  .carousel-item {
    min-height: 360px;
    height: 80vh;
  }

  .carousel-caption {
    padding: 0.8rem;
  }

  .hero-title {
    font-size: 1.6rem;
  }

  .hero-subtitle {
    font-size: 0.9rem;
  }

  .hero-description {
    font-size: 0.85rem;
    padding: 0 0.5rem;
  }

  .stats-container {
    flex-direction: column;
    align-items: center;
    gap: 0.8rem;
  }

  .stat-item {
    display: flex;
    align-items: center;
    gap: 0.8rem;
    min-width: auto;
  }

  .stat-icon {
    font-size: 1.5rem;
    margin: 0;
  }

  .stat-number {
    font-size: 1.2rem;
    margin: 0;
  }

  .stat-label {
    font-size: 0.75rem;
    margin: 0;
  }

  .hero-buttons {
    padding: 0 0.5rem;
  }

  .hero-buttons .btn {
    font-size: 0.85rem;
    padding: 0.6rem 1.5rem;
  }

  /* Features Section */
  .features {
    padding: 2.5rem 0.8rem;
  }

  .feature-card {
    padding: 1.2rem;
  }

  .feature-card h3 {
    font-size: 1.1rem;
  }

  .feature-card p {
    font-size: 0.85rem;
  }

  .feature-icon {
    font-size: 2.5rem;
  }

  /* Services Section */
  .services {
    padding: 3rem 0.8rem;
  }

  .service-categories {
    gap: 0.4rem;
    justify-content: center;
  }

  .category-btn {
    font-size: 0.75rem;
    padding: 0.4rem 0.8rem;
    border-radius: 20px;
  }

  .section-title {
    font-size: 1.6rem;
  }

  .section-subtitle {
    font-size: 0.9rem;
  }

  .services-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 1rem;
  }

  .service-card {
    border-radius: 12px;
    box-shadow: 0 4px 15px rgba(120, 186, 126, 0.1);
  }

  .service-image {
    height: 90px;
  }

  .service-content {
    padding: 0.8rem;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
  }

  .service-title {
    font-size: 0.9rem;
    margin-bottom: 0.4rem;
    line-height: 1.3;
  }

  .service-meta {
    flex-direction: column;
    align-items: flex-start;
    gap: 0.2rem;
    margin-bottom: 0.5rem;
  }

  .service-duration {
    font-size: 0.65rem;
    padding: 0.15rem 0.5rem;
  }

  .service-price {
    font-size: 0.85rem;
  }

  .service-description {
    font-size: 0.75rem;
    line-height: 1.3;
    margin-bottom: 0.5rem;
  }

  .service-book-btn {
    font-size: 0.8rem;
    padding: 0.5rem 0.8rem;
    border-radius: 15px;
    margin-top: 0;
    align-self: center;
  }

  .service-rating-overlay {
    top: 0.2rem;
    right: 0.2rem;
    padding: 0.15rem 0.3rem;
  }

  .service-rating {
    font-size: 0.65rem;
  }

  .rating-star {
    font-size: 0.7rem;
  }

  .rating-text {
    font-size: 0.6rem;
  }

  /* Testimonials Section */
  .testimonials {
    padding: 3rem 0.8rem;
  }

  .testimonial-card {
    padding: 1rem;
  }

  .testimonial-text {
    font-size: 0.9rem;
  }

  .testimonial-author {
    font-size: 0.85rem;
  }

  .testimonial-rating i {
    font-size: 0.8rem;
  }

  /* Booking Section */
  .booking {
    padding: 3rem 0.8rem;
  }

  .booking-info h2 {
    font-size: 1.8rem;
  }

  .booking-info p {
    font-size: 0.9rem;
  }

  .booking-benefits li {
    font-size: 0.85rem;
  }

  .booking-form {
    padding: 1.2rem;
  }

  .form-group label {
    font-size: 0.85rem;
  }

  .form-group input,
  .form-group select,
  .form-group textarea {
    padding: 0.7rem;
    font-size: 0.9rem;
  }

  .form-actions {
    flex-direction: column;
    align-items: stretch;
    gap: 0.8rem;
  }

  .add-service-btn {
    font-size: 0.85rem;
    padding: 0.6rem 1.2rem;
  }

  .consult-checkbox {
    font-size: 0.85rem;
  }

  .selected-services h4 {
    font-size: 1rem;
  }

  .selected-service-item {
    padding: 0.6rem;
  }

  .service-name {
    font-size: 0.9rem;
  }

  .service-price {
    font-size: 0.8rem;
  }

  .quantity-input {
    width: 40px;
    font-size: 0.8rem;
  }

  .remove-service-btn {
    width: 24px;
    height: 24px;
    font-size: 1rem;
  }

  .submit-btn {
    font-size: 0.95rem;
    padding: 0.9rem 2rem;
  }

  /* About Section */
  .about-section {
    padding: 3rem 0.8rem;
  }

  .about-section h2 {
    font-size: 1.8rem;
  }

  .about-list li {
    font-size: 0.9rem;
  }

  .about-image-box {
    padding: 1.5rem;
  }

  .about-image-box img {
    max-width: 80%;
  }

  /* Carousel indicators */
  .carousel-indicators {
    bottom: 10px;
  }

  .carousel-indicators button {
    width: 8px;
    height: 8px;
    margin: 0 2px;
  }
}

/* Mobile Extra Small */
@media (max-width: 400px) {
  .carousel-item {
    height: 75vh;
    min-height: 450px;
  }

  .carousel-caption {
    top: 40%;
  }

  .display-2 {
    font-size: 1.2rem !important;
  }

  .fs-3 {
    font-size: 0.8rem !important;
  }

  .lead {
    font-size: 0.75rem !important;
  }

  .d-flex.flex-wrap.justify-content-center.gap-4.mb-5 > div {
    max-width: 260px;
    padding: 0.5rem 0.8rem;
  }

  .btn-lg {
    padding: 0.6rem 1.2rem !important;
    font-size: 0.75rem !important;
  }
}
.detail-service-link {
  text-decoration: none;
}
</style>
