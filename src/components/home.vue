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
                <button
                  @click="scrollToServices"
                  class="btn btn-primary btn-lg rounded-pill px-5 py-3 fw-bold"
                >
                  Khám phá dịch vụ
                </button>
                <button
                  @click="scrollToBooking"
                  class="btn btn-outline-light btn-lg rounded-pill px-5 py-3"
                >
                  Đặt lịch ngay
                </button>
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
          <div class="feature-card" v-for="(feature, index) in features" :key="index">
            <div class="feature-icon">{{ feature.icon }}</div>
            <h3>{{ feature.title }}</h3>
            <p>{{ feature.description }}</p>
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
               <router-link class="detail-service-link" :to="`/DichVuChiTiet/${service.id}`"> 
              <img
                :src="service.image"
                :alt="service.name"
                class="service-image"
                loading="lazy"
              />
                </router-link>
              <div class="service-rating-overlay" v-if="service.rating">
                <div class="service-rating">
                  <i
                    v-for="n in 5"
                    :key="n"
                    class="rating-star"
                    :class="
                      n <= Math.floor(service.rating)
                        ? 'filled'
                        : n - 0.5 <= service.rating
                        ? 'half-filled'
                        : ''
                    "
                    >★</i
                  >
                  <span class="rating-text"
                    >({{ service.rating?.toFixed(1) || "0.0" }})</span
                  >
                </div>
              </div>
            </div>
            <div class="service-content">
              <router-link class="detail-service-link" :to="`/DichVuChiTiet/${service.id}`  "> 
              <h3 class="service-title">{{ service.name }}</h3>
              <div class="service-meta">
                <div class="service-duration">
                  <i class="duration-icon">⏱</i>
                  {{ service.duration }} phút
                </div>
                <div class="service-price">{{ service.price }} VNĐ</div>
              </div>
              </router-link> 
              <button
                class="service-book-btn"
                @click="addServiceFromCard(service)"
                :disabled="bookingForm.consultAtStore"
              >
                <span>Đặt lịch ngay</span>
                <i class="btn-arrow">→</i>
              </button>
              
            </div>

          </div>
          
        </div>
        
      </div>
      
    </section>

    <!-- Testimonials Section -->
    <section class="testimonials-section">
      <div class="container">
        <h2 class="section-title text-center mb-5">
          Khách hàng nói gì về chúng tôi
        </h2>

        <div v-if="testimonialsLoading" class="text-center text-white">
          <div class="loading-spinner"></div>
          <p>Đang tải đánh giá...</p>
        </div>
        
        <div v-else-if="testimonials.length === 0" class="text-center text-white opacity-75">
          <div class="no-reviews-icon">💬</div>
          <p class="mt-3">Chưa có đánh giá nào. Hãy là người đầu tiên chia sẻ trải nghiệm của bạn!</p>
        </div>

        <div v-else class="testimonials-container">
          <div class="testimonials-grid">
            <div
              v-for="(item, index) in visibleTestimonials"
              :key="item.id || index"
              class="testimonial-card"
              :class="{ 'testimonial-featured': index === 0 }"
            >
              <div class="testimonial-content">
                <div class="testimonial-header">
                  <div class="testimonial-avatar">
                    <span>{{ getInitials(item.user?.name || 'Khách hàng') }}</span>
                  </div>
                  <div class="testimonial-info">
                    <h4 class="testimonial-name">
                      {{ item.anDanh ? 'Ẩn danh' : item.user?.name || 'Khách hàng' }}
                    </h4>
                    <div class="testimonial-rating">
                      <i
                        v-for="n in 5"
                        :key="n"
                        class="fa-star fas"
                        :class="n <= item.soSao ? 'text-warning' : 'text-secondary opacity-25'"
                      ></i>
                    </div>
                  </div>
                </div>
                <p class="testimonial-text">
                  "{{ item.noiDung || 'Dịch vụ tuyệt vời, tôi rất hài lòng!' }}"
                </p>
                <div class="testimonial-footer">
                  <span class="testimonial-age">{{ item.user?.tuoi || '...' }} tuổi</span>
                  <div class="testimonial-decoration"></div>
                </div>
              </div>
            </div>
          </div>

          <div v-if="testimonials.length > 3" class="text-center mt-4">
            <button 
              class="show-more-btn"
              @click="toggleShowAll"
            >
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

          <div v-if="showAllTestimonials && testimonials.length > 3" class="additional-testimonials">
            <div class="testimonials-grid mt-4">
              <div
                v-for="(item, index) in additionalTestimonials"
                :key="item.id || (index + 3)"
                class="testimonial-card testimonial-fade-in"
              >
                <div class="testimonial-content">
                  <div class="testimonial-header">
                    <div class="testimonial-avatar">
                      <span>{{ getInitials(item.user?.name || 'Khách hàng') }}</span>
                    </div>
                    <div class="testimonial-info">
                      <h4 class="testimonial-name">
                        {{ item.anDanh ? 'Ẩn danh' : item.user?.name || 'Khách hàng' }}
                      </h4>
                      <div class="testimonial-rating">
                        <i
                          v-for="n in 5"
                          :key="n"
                          class="fa-star fas"
                          :class="n <= item.soSao ? 'text-warning' : 'text-secondary opacity-25'"
                        ></i>
                      </div>
                    </div>
                  </div>
                  <p class="testimonial-text">
                    "{{ item.noiDung || 'Dịch vụ tuyệt vời, tôi rất hài lòng!' }}"
                  </p>
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
                required
                placeholder="0123 456 789"
                pattern="[0-9]{10,11}"
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
                  Thêm dịch vụ
                </button>
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
                  :key="service.id + '_' + index"
                  class="selected-service-card"
                >
                  <div class="service-info">
                    <span class="service-name">{{ service.name }}</span>
                    <span class="service-price">{{ service.price.toLocaleString() }} VNĐ</span>
                  </div>
                  <div class="service-actions">
                    <label :for="'quantity-' + index" class="quantity-label">Số lượng:</label>
                    <input
                      type="number"
                      :id="'quantity-' + index"
                      v-model.number="service.soLuong"
                      min="1"
                      max="10"
                      required
                      class="quantity-input"
                    />
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
                <select id="time" v-model="bookingForm.time" required>
                  <option value="">-- Chọn giờ --</option>
                  <option
                    v-for="slot in availableSlots"
                    :key="slot.khungGio"
                    :value="slot.khungGio"
                  >
                    {{ slot.khungGio }}
                    <span v-if="slot.conLai <= 2"
                      >(Còn {{ slot.conLai }} chỗ)</span
                    >
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
                maxlength="500"
              ></textarea>
            </div>

            <button
              type="submit"
              class="submit-btn"
              :disabled="isSubmitting || !isFormValid"
            >
              {{ isSubmitting ? 'Đang xử lý...' : 'Đặt lịch hẹn' }}
            </button>
          </form>
        </div>
      </div>
    </section>

    <!-- About Section -->
    <section id="about" class="about-section">
      <div class="container">
        <div class="row align-items-center">
          <div class="col-lg-6">
            <div class="about-content-wrapper">
              <h2 class="about-title">
                <span class="title-accent">Về</span> TutaSpa
              </h2>
              <div class="about-subtitle">
                Điểm đến hoàn hảo cho sự thư giãn và làm đẹp
              </div>
              <p class="about-description">
                Với hơn <strong>10 năm kinh nghiệm</strong> trong ngành làm đẹp, TutaSpa tự hào mang đến 
                cho khách hàng những trải nghiệm thư giãn tuyệt vời nhất. Chúng tôi kết hợp tinh hoa 
                truyền thống với công nghệ hiện đại, sử dụng 100% sản phẩm từ thiên nhiên.
              </p>

              <div class="about-features">
                <div class="feature-item" v-for="(feature, index) in aboutFeatures" :key="index">
                  <div class="feature-icon-wrapper">
                    <span class="feature-emoji">{{ feature.emoji }}</span>
                  </div>
                  <div class="feature-content">
                    <h4>{{ feature.title }}</h4>
                    <p>{{ feature.description }}</p>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <div class="col-lg-6">
            <div class="about-visual">
              <div class="about-image-main">
                <img
                  src="https://images.unsplash.com/photo-1516975080664-ed2fc6a32937?ixlib=rb-4.0.3&auto=format&fit=crop&w=2070&q=80"
                  alt="TutaSpa Interior"
                  class="img-fluid rounded-4 shadow-lg"
                  loading="lazy"
                />
                <div class="image-overlay">
                  <div class="overlay-content">
                    <h3>Không gian thư giãn</h3>
                    <p>Thiết kế sang trọng, yên tĩnh</p>
                  </div>
                </div>
              </div>
              
              <div class="about-stats">
                <div class="stat-item" v-for="(stat, index) in aboutStats" :key="index">
                  <div class="stat-number">{{ stat.number }}</div>
                  <div class="stat-label">{{ stat.label }}</div>
                </div>
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

// Reactive state
const services = ref([]);
const categories = ref([]);
const currentCategory = ref("all");
const filteredServices = ref([]);
const loading = ref(true);
const testimonialsLoading = ref(true);
const error = ref(null);
const minDate = ref("");
const selectedService = ref(null);
const isSubmitting = ref(false);
const showAllTestimonials = ref(false);
const testimonials = ref([]);
const availableSlots = ref([]);
const currentSlide = ref(0);

const bookingForm = ref({
  phone: "",
  services: [],
  date: new Date().toISOString().split("T")[0],
  time: "",
  notes: "",
  consultAtStore: false,
});

// Constants
const IMAGE_BASE_URL = import.meta.env.VITE_BASE_URL.replace("/api", "") + "/images/";

const slides = [
  {
    image: "https://images.unsplash.com/photo-1540555700478-4be289fbecef?ixlib=rb-4.0.3&auto=format&fit=crop&w=2070&q=80",
    title: "Thư Giãn & Tái Tạo",
    subtitle: "Trải nghiệm không gian yên bình",
  },
  {
    image: "https://images.unsplash.com/photo-1596178065887-1198b6148b2b?ixlib=rb-4.0.3&auto=format&fit=crop&w=2070&q=80",
    title: "Chăm Sóc Chuyên Nghiệp",
    subtitle: "Với đội ngũ chuyên gia giàu kinh nghiệm",
  },
  {
    image: "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?ixlib=rb-4.0.3&auto=format&fit=crop&w=2070&q=80",
    title: "Làm Đẹp Tự Nhiên",
    subtitle: "Sử dụng 100% sản phẩm thiên nhiên",
  },
];

const stats = [
  { number: "10+", label: "Năm kinh nghiệm", icon: "🏆" },
  { number: "5000+", label: "Khách hàng hài lòng", icon: "👥" },
  { number: "24", label: "Dịch vụ chuyên nghiệp", icon: "🌿" },
];

const features = [
  {
    icon: "🌱",
    title: "100% Tự nhiên",
    description: "Sử dụng các sản phẩm từ thiên nhiên, không chất hóa học có hại, an toàn cho mọi loại da"
  },
  {
    icon: "👥",
    title: "Chuyên gia giàu kinh nghiệm",
    description: "Đội ngũ chuyên viên được đào tạo bài bản, và có kinh nghiệm hơn 5 năm"
  },
  {
    icon: "🏛️",
    title: "Không gian sang trọng",
    description: "Thiết kế hiện đại, thoáng mát với âm nhạc thư giãn và hương thơm dễ chịu"
  },
  {
    icon: "🛡️",
    title: "Đảm bảo vệ sinh",
    description: "Tuân thủ nghiêm ngặt các tiêu chuẩn vệ sinh, khử trùng dụng cụ sau mỗi lần sử dụng"
  }
];

const aboutFeatures = [
  {
    emoji: "🌿",
    title: "Sản phẩm hữu cơ cao cấp",
    description: "Nhập khẩu trực tiếp từ các thương hiệu uy tín"
  },
  {
    emoji: "👩‍⚕️",
    title: "Đội ngũ chuyên gia quốc tế",
    description: "Được đào tạo bài bản tại Hàn Quốc và Nhật Bản"
  },
  {
    emoji: "🏆",
    title: "Chứng nhận ISO 9001:2015",
    description: "Đảm bảo chất lượng dịch vụ quốc tế"
  },
  {
    emoji: "💯",
    title: "Cam kết hài lòng 100%",
    description: "Hoàn tiền nếu không hài lòng với dịch vụ"
  }
];

const aboutStats = [
  { number: "5000+", label: "Khách hàng tin tưởng" },
  { number: "50+", label: "Dịch vụ chuyên nghiệp" },
  { number: "98%", label: "Tỷ lệ hài lòng" }
];

// Computed properties
const visibleTestimonials = computed(() => testimonials.value.slice(0, 3));
const additionalTestimonials = computed(() => testimonials.value.slice(3));

const isFormValid = computed(() => {
  return (
    bookingForm.value.phone &&
    (bookingForm.value.services.length > 0 || bookingForm.value.consultAtStore) &&
    bookingForm.value.date &&
    bookingForm.value.time
  );
});

// Methods
const scrollToServices = () => {
  document.getElementById('services')?.scrollIntoView({ behavior: 'smooth' });
};

const scrollToBooking = () => {
  document.getElementById('booking')?.scrollIntoView({ behavior: 'smooth' });
};

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
  scrollToBooking();
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

// Watch for date changes to fetch available slots
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
    if (categories.value.length === 0) throw new Error("Không có danh mục dịch vụ nào");
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
          categories.value.find((cat) => cat.loaiDichVuID === service.loaiDichVuID)?.tenLoai || "Khác",
        price: service.gia.toLocaleString("vi-VN"),
        duration: service.thoiGian,
        description: service.moTa,
        image: `${IMAGE_BASE_URL}${service.hinhAnh}`,
        rating: service.danhGiaTrungBinh || 0
      }));
    if (services.value.length === 0) throw new Error("Không có dịch vụ nào");
  } catch (err) {
    console.error("Lỗi khi tải dịch vụ:", err);
    error.value = "Không thể tải dịch vụ. Hiển thị mặc định.";
    services.value = [
      {
        id: 1,
        name: "Triệt lông full chân",
        category: "Triệt lông",
        price: "500.000",
        duration: 45,
        description: "Triệt lông SHR không đau rát, hiệu quả cao",
        image: `${IMAGE_BASE_URL}triet_long_full_chan.jpg`,
        rating: 4.5
      },
    ];
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
    selectedService.value = null;
  }
};

const removeService = (index) => {
  bookingForm.value.services.splice(index, 1);
};

const submitBooking = async () => {
  if (!isFormValid.value || isSubmitting.value) return;
  
  isSubmitting.value = true;
  try {
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
          soLuong: s.soLuong,
        }));

    const payload = {
      soDienThoai: bookingForm.value.phone,
      thoiGian,
      dichVus,
      ghiChu: bookingForm.value.notes,
      datTruoc: true,
    };

    await apiClient.post("/DatLich", payload);
    alert("Đặt lịch thành công! Chúng tôi sẽ liên hệ với bạn sớm nhất.");
    resetBookingForm();
  } catch (err) {
    console.error("Lỗi đặt lịch:", err);
    const errorMessage = err.response?.data?.message || "Đặt lịch thất bại. Vui lòng thử lại!";
    alert(errorMessage);
  } finally {
    isSubmitting.value = false;
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

const fetchTestimonials = async () => {
  testimonialsLoading.value = true;
  try {
    const res = await apiClient.get("/DanhGia/admin");
    testimonials.value = res
      .filter((dg) => dg.daDuyet && dg.isActive)
      .sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt))
      .slice(0, 20); // Limit to 20 testimonials for performance
  } catch (err) {
    console.error("Lỗi khi tải đánh giá:", err);
    testimonials.value = [];
  } finally {
    testimonialsLoading.value = false;
  }
};

// Lifecycle hooks
onMounted(async () => {
  try {
    minDate.value = new Date().toISOString().split("T")[0];
    loading.value = true;

    // Load data concurrently for better performance
    await Promise.all([
      fetchCategories(),
      fetchServices(),
      fetchSlots(),
      fetchTestimonials()
    ]);

    filterServices("all");
  } catch (err) {
    console.error("Lỗi khi khởi tạo:", err);
    error.value = "Có lỗi xảy ra khi tải dữ liệu. Vui lòng tải lại trang.";
  } finally {
    loading.value = false;
  }
});
</script>

<style scoped>
/* Hero Section */
.carousel-item {
  height: 100vh;
  background-size: cover !important;
  background-position: center !important;
}

.carousel-caption {
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

.font-lora {
  font-family: "Lora", serif;
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
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
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
  color: #2d4a2d;
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
  font-weight: bold;
  color: #f59e0b;
  font-family: "Lora", serif;
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
  margin-top: auto;
}

.service-book-btn:hover:not(:disabled) {
  transform: scale(1.05);
  box-shadow: 0 6px 20px rgba(120, 186, 126, 0.5);
}

.service-book-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

.btn-arrow {
  font-size: 1.2rem;
  transition: transform 0.3s ease;
}

.service-book-btn:hover:not(:disabled) .btn-arrow {
  transform: translateX(3px);
}

/* Loading States */
.loading {
  text-align: center;
  padding: 4rem 2rem;
}

.loading-spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid #78ba7e;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 1rem;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.error {
  text-align: center;
  padding: 2rem;
  color: #dc2626;
  background: #fef2f2;
  border-radius: 10px;
  margin: 2rem 0;
}

/* Testimonials Section */
.testimonials-section {
  padding: 8rem 2rem;
  background: linear-gradient(135deg, #cef8d2 20%, #6ba371 50%, #95dda0 100%);
  position: relative;
  overflow: hidden;
}

.testimonials-section::before {
  content: '';
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
  content: '';
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
  border: 2px solid #fbbf24;
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
  padding: 0;
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

.submit-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 10px 25px rgba(245, 158, 11, 0.4);
}

.submit-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

.add-service-btn {
  background: linear-gradient(45deg, #78ba7e, #5e8c64);
  color: white;
  padding: 0.8rem 2rem;
  border: none;
  border-radius: 25px;
  margin-top: 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.add-service-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(120, 186, 126, 0.4);
}

.add-service-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.consult-checkbox {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: white;
  font-size: 0.9rem;
  cursor: pointer;
}

.consult-checkbox input[type="checkbox"] {
  width: 15px !important;
  height: 15px !important;
  cursor: pointer;
}

.selected-services {
  margin-bottom: 2rem;
}

.selected-services .section-title {
  font-size: 1.2rem;
  font-weight: 600;
  color: white;
  margin-bottom: 1rem;
  text-align: left;
}

.selected-services .section-title::after {
  display: none;
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
  background: rgba(255, 255, 255, 0.9);
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
  color: #2d4a2d;
}

.service-price {
  color: #4a8c4a;
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
  width: 60px !important;
  padding: 0.3rem !important;
  border: 1px solid #ddd !important;
  border-radius: 6px !important;
  text-align: center;
  background: white !important;
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
  display: flex;
  align-items: center;
  justify-content: center;
}

.remove-service-btn:hover {
  background: #e60000;
}

/* About Section */
.about-section {
  padding: 6rem 2rem;
  background: linear-gradient(135deg, #f8fdf8 0%, #f0fdf4 50%, #e8f5e8 100%);
  position: relative;
  overflow: hidden;
}

.about-section::before {
  content: '';
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
.detail-service-link{
  text-decoration: none;

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

/* Responsive Design */
@media (max-width: 768px) {
  .carousel-item {
    height: 70vh;
  }
  
  .display-2 {
    font-size: 2rem !important;
  }
  
  .features-grid {
    grid-template-columns: 1fr;
    gap: 2rem;
  }
  
  .services-grid {
    grid-template-columns: 1fr;
  }
  
  .service-categories {
    flex-direction: column;
    align-items: center;
  }
  
  .booking-content {
    grid-template-columns: 1fr;
    gap: 3rem;
  }
  
  .form-row {
    grid-template-columns: 1fr;
  }
  
  .testimonials-grid {
    grid-template-columns: 1fr;
  }
  
  .about-stats {
    flex-direction: column;
    gap: 1.5rem;
    text-align: center;
  }
  
  .about-title {
    font-size: 2.5rem;
  }
  
  .about-features {
    gap: 1.5rem;
  }
  
  .feature-item {
    padding: 1rem;
  }
  
  .feature-item:hover {
    transform: translateY(-5px) translateX(0);
  }
}

@media (max-width: 480px) {
  .carousel-item {
    height: 60vh;
  }
  
  .display-2 {
    font-size: 1.5rem !important;
  }
  
  .section-title {
    font-size: 2rem;
  }
  
  .service-card {
    margin: 0 1rem;
  }
  
  .booking-form {
    padding: 2rem;
  }
  
  .about-title {
    font-size: 2rem;
  }
}

/* Accessibility improvements */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

/* Focus styles for keyboard navigation */
.category-btn:focus,
.service-book-btn:focus,
.submit-btn:focus,
.add-service-btn:focus {
  outline: 2px solid #fbbf24;
  outline-offset: 2px;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  outline: 2px solid #78ba7e;
  outline-offset: 2px;
}

/* High contrast mode support */
@media (prefers-contrast: high) {
  .service-card {
    border: 2px solid #000;
  }
  
  .testimonial-card {
    border: 2px solid #000;
  }
  
  .category-btn {
    border: 2px solid #000;
  }
}
</style>