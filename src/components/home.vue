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
                class="hero-title fw-bold mb-4 font-lora animate__animated animate__fadeInDown"
              >
                Chào mừng đến TutaSpa
              </h1>
              <h2
                class="hero-subtitle fw-semibold text-white mb-4 animate__animated animate__fadeInUp animate__delay-1s"
              >
                {{ slides[currentSlide].subtitle }}
              </h2>
              <p
                class="hero-description text-white mb-5 mx-auto animate__animated animate__fadeInUp animate__delay-2s"
              >
                Hành trình thư giãn và tái tạo năng lượng với những liệu pháp tự
                nhiên, mang đến sự cân bằng hoàn hảo cho cơ thể và tâm hồn bạn
              </p>
              <div
                class="stats-container d-flex flex-wrap justify-content-center gap-4 mb-5 animate__animated animate__fadeInUp animate__delay-3s"
              >
                <div
                  v-for="(stat, index) in stats"
                  :key="index"
                  class="stat-item text-center"
                >
                  <div class="stat-icon">{{ stat.icon }}</div>
                  <div class="stat-number">{{ stat.number }}</div>
                  <div class="stat-label">{{ stat.label }}</div>
                </div>
              </div>
              <div
                class="hero-buttons d-flex flex-column flex-sm-row justify-content-center gap-3 animate__animated animate__fadeInUp animate__delay-4s"
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
          Đội ngũ chuyên viên được đào tạo bài bản, và có kinh nghiệm hơn 5 năm
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
      <!-- Mục mới 1 -->
      <div class="feature-card">
        <div class="feature-icon">💆</div>
        <h3>Liệu trình đa dạng</h3>
        <p>
          Cung cấp nhiều liệu trình chăm sóc phù hợp với nhu cầu và tình trạng da khác nhau
        </p>
      </div>
      <!-- Mục mới 2 -->
      <div class="feature-card">
        <div class="feature-icon">⏱️</div>
        <h3>Đặt lịch linh hoạt</h3>
        <p>
          Dễ dàng đặt lịch hẹn theo thời gian bạn mong muốn, kể cả cuối tuần và ngày lễ
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
              <router-link :to="`/DichVuChiTiet/${service.id}`"> 
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
                    :class="n <= Math.floor(service.rating) ? 'filled' : 
                             n - 0.5 <= service.rating ? 'half-filled' : ''"
                  >★</i>
                  <span class="rating-text">({{ service.rating?.toFixed(1) || '0.0' }})</span>
                </div>
              </div>
            </div>
            <div class="service-content">
              <h3 class="service-title">{{ service.name }}</h3>
              <div class="service-meta">
                <div class="service-duration">
                  <i class="duration-icon">⏱</i>
                  {{ service.duration }} phút
                </div>
                <div class="service-price">{{ service.price }} VNĐ</div>
              </div>
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

    <!-- Testimonials -->
    <section class="testimonials py-5 bg-light">
      <div class="container">
        <h2 class="section-title text-center mb-5">
          Khách hàng nói gì về chúng tôi
        </h2>

        <div v-if="loading" class="text-center text-muted">Đang tải...</div>
        <div v-else-if="testimonials.length === 0" class="text-center text-muted">
          Chưa có đánh giá nào.
        </div>

        <div v-else class="testimonials-grid row g-4">
          <div
            v-for="(item, index) in testimonials"
            :key="index"
            class="testimonial-card col-md-4"
          >
            <div class="testimonial-rating mb-2">
              <i
                v-for="n in 5"
                :key="n"
                class="fa-star fas"
                :class="n <= item.rate ? 'text-warning' : 'text-secondary opacity-25'"
              ></i>
            </div>
            <p class="testimonial-text fst-italic">
              "{{ item.content || '(Không có nội dung)' }}"
            </p>
            <div class="testimonial-author mt-2 fw-semibold">
              - 
              <span >{{ item.name || 'Khách hàng' }}</span>
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
              <div class="form-actions">
                <button
                  type="button"
                  class="add-service-btn"
                  :disabled="!selectedService || bookingForm.consultAtStore"
                  @click="addService"
                >
                  Thêm dịch vụ
                </button>
                <!-- Checkbox tư vấn tại quán -->
                <label class="consult-checkbox">
                  <input
                    type="checkbox"
                    v-model="bookingForm.consultAtStore"
                    @change="handleConsultAtStore"
                  />
                  <span class="checkmark"></span>
                  Tới quán nhân viên tư vấn
                </label>
              </div>
            </div>

            <!-- Selected Services List -->
            <div
              class="selected-services"
              v-if="bookingForm.services.length && !bookingForm.consultAtStore"
            >
              <h4>Dịch vụ đã chọn:</h4>
              <ul>
                <li
                  v-for="(service, index) in bookingForm.services"
                  :key="index"
                  class="selected-service-item"
                >
                  <div class="service-info">
                    <span class="service-name">{{ service.name }}</span>
                    <span class="service-price">{{ service.price }} VNĐ</span>
                  </div>
                  <div class="quantity-control">
                    <label :for="'quantity-' + index">SL:</label>
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
                    >
                      ×
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
              ></textarea>
            </div>

            <button
              type="submit"
              class="submit-btn"
              :disabled="
                !bookingForm.phone ||
                (!bookingForm.services.length && !bookingForm.consultAtStore) ||
                !bookingForm.date ||
                !bookingForm.time
              "
            >
              <span>Đặt lịch hẹn</span>
              <i class="btn-icon">📅</i>
            </button>
          </form>
        </div>
      </div>
    </section>

    <!-- About Section -->
    <section id="about" class="about-section py-5">
      <div class="container">
        <div class="row align-items-center">
          
          <!-- Cột trái: Nội dung -->
          <div class="col-lg-6">
            <h2 class="fw-bold mb-4">Về TutaSpa</h2>
            <p class="mb-4">
              Hãy để TutaSpa mang đến cho bạn những phút giây thư giãn tuyệt vời cùng dịch vụ làm đẹp chuyên nghiệp, kết hợp tinh hoa truyền thống và công nghệ hiện đại.
            </p>

            <ul class="list-unstyled about-list">
              <li>🌿 Sản phẩm hữu cơ cao cấp</li>
              <li>💆‍♀️ Đội ngũ chuyên gia quốc tế</li>
              <li>🏆 Chứng nhận ISO 9001:2015</li>
              <li>💯 Cam kết hài lòng 100%</li>
            </ul>
          </div>

          <!-- Cột phải: Hình ảnh -->
          <div class="col-lg-6 text-center">
            <div class="about-image-box p-3 rounded">
              <img
                src="../assets/img/logotutaspa.png"
                alt="TutaSpa"
                class="img-fluid rounded shadow"
              />
            </div>
          </div>

        </div>
      </div>
    </section>

  </div>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";
  import apiClient from "../utils/axiosClient";

// Reactive state
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
      "https://images.unsplash.com/photo-1596178065887-1198b6148b2b?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80",
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

const testimonials = ref([]);

onMounted(async () => {
  try {
    const res = await apiClient.get("/DanhGia/approved");
    testimonials.value = res
  } catch (err) {
    console.error("Lỗi khi tải testimonials:", err);
  } finally {
    loading.value = false;
  }
});

const stats = [
  { number: "10+", label: "Năm kinh nghiệm", icon: "🏆" },
  { number: "5000+", label: "Khách hàng hài lòng", icon: "👥" },
  { number: "24", label: "Dịch vụ chuyên nghiệp", icon: "🌿" },
];

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
    bookingForm.value.services.push({
      ...service,
      soLuong: 1,
    });
  }
  // Scroll to booking section
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

// Gọi API khi đổi ngày
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
    if (categories.value.length === 0) {
      throw new Error("Không có danh mục dịch vụ nào được trả về");
    }
  } catch (err) {
    console.error("Lỗi khi tải danh mục:", err);
    error.value = "Không thể tải danh mục dịch vụ. Hiển thị danh mục mặc định.";
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
    console.log("Dịch vụ:", data.length);
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
        rating: service.mucDanhGia || 0, // Thêm rating từ API
        image: `${IMAGE_BASE_URL}${service.hinhAnh}`,
      }));
  } catch (err) {
    console.error("Lỗi khi tải dịch vụ:", err);
    error.value = "Không thể tải danh sách dịch vụ. Hiển thị dịch vụ mặc định.";
    services.value = [
      {
        id: 1,
        name: "Triệt lông full chân",
        category: "Triệt lông",
        price: "500.000",
        duration: 45,
        description:
          "Triệt lông vùng chân bằng công nghệ ánh sáng SHR không đau rát, hiệu quả cao",
        rating: 4.5,
        image: `${IMAGE_BASE_URL}triet_long_full_chan.jpg`,
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
    selectedService.value = null; // Reset dropdown
  }
};

const removeService = (index) => {
  bookingForm.value.services.splice(index, 1);
};

const submitBooking = async () => {
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

    const res = await apiClient.post("/DatLich", payload);

    alert("Đặt lịch thành công!");
    bookingForm.value = {
      phone: "",
      services: [],
      date: new Date().toISOString().split("T")[0],
      time: "",
      notes: "",
      consultAtStore: false,
    };
    selectedService.value = null;
  } catch (err) {
    console.error("Lỗi đặt lịch:", err);
    alert("Đặt lịch thất bại!");
  }
};

const resetBookingForm = () => {
  bookingForm.value = {
    phone: "",
    services: [],
    date: "",
    time: "",
    notes: "",
    consultAtStore: false,
  };
  selectedService.value = null;
};

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

// Lifecycle hook
onMounted(async () => {
  minDate.value = new Date().toISOString().split("T")[0];
  loading.value = true;
  await fetchCategories();
  await fetchServices();
  loading.value = false;
  filterServices("all");
  fetchSlots();
});
</script>

<style>
/* Base styles */
* {
  box-sizing: border-box;
}

/* Hero Section Responsive */
.carousel-item {
  height: 100vh;
  background-size: cover !important;
  background-position: center !important;
  min-height: 500px;
}

.carousel-caption {
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
  padding: 2rem;
}

.hero-title {
  font-size: 3.5rem;
  line-height: 1.2;
}

.hero-subtitle {
  font-size: 1.5rem;
}

.hero-description {
  max-width: 700px;
  font-size: 1.1rem;
  line-height: 1.6;
}

.stats-container {
  gap: 2rem;
}

.stat-item {
  min-width: 120px;
}

.stat-icon {
  font-size: 2.5rem;
  margin-bottom: 0.5rem;
}

.stat-number {
  font-size: 2rem;
  font-weight: bold;
  color: #fbbf24;
  margin-bottom: 0.25rem;
}

.stat-label {
  font-size: 0.9rem;
  opacity: 0.9;
}

.hero-buttons {
  gap: 1rem;
}

.font-lora {
  font-family: "Lora", serif;
}

/* Features Section */
.features {
  padding: 6rem 2rem;
  background: white;
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 1rem;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

.feature-card {
  text-align: center;
  padding: 2rem;
  border-radius: 15px;
  background: linear-gradient(135deg, #f0fdf4 0%, #dcfce7 100%);
  transition: all 0.3s ease;
  height: 100%;
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

.feature-card p {
  font-size: 1rem;
  line-height: 1.6;
  color: #4b5563;
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
  grades: 1.3rem;
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

/* Testimonials */
.testimonials {
  background: #f8f9fa;
}

.testimonials-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 1.5rem;
}

.testimonial-card {
  background: white;
  padding: 1.5rem;
  border-radius: 1rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
  transition: transform 0.2s, box-shadow 0.3s;
  flex: 1;
  min-width: 300px;
}

.testimonial-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.08);
}

.testimonial-rating i {
  margin-right: 2px;
}

.testimonial-text {
  font-size: 1rem;
  line-height: 1.6;
  margin: 1rem 0;
}

/* Booking Section */
.booking {
  padding: 8rem 2rem;
  background: linear-gradient(135deg, #78ba7e 0%, #6ba371 50%, #5e8c64 100%);
  color: white;
}

.booking-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  max-width: 1200px;
  margin: 0 auto;
  align-items: start;
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
  line-height: 1.6;
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
  gap: 0.75rem;
  font-size: 1rem;
}

.booking-benefits li::before {
  content: "✓";
  color: #fbbf24;
  font-weight: bold;
  font-size: 1.2rem;
  flex-shrink: 0;
}

.booking-form {
  background: rgba(255, 255, 255, 0.15);
  padding: 2.5rem;
  border-radius: 20px;
  backdrop-filter: blur(10px);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 600;
  color: white;
  font-size: 0.95rem;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 0.9rem;
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

.form-actions {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 1rem;
  margin-top: 1rem;
}

.add-service-btn {
  background: linear-gradient(45deg, #78ba7e, #5e8c64);
  color: white;
  padding: 0.8rem 1.5rem;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  font-weight: 600;
  transition: all 0.3s ease;
  font-size: 0.9rem;
}

.add-service-btn:disabled {
  background: #d1d5db;
  cursor: not-allowed;
}

.add-service-btn:hover:not(:disabled) {
  transform: scale(1.05);
  box-shadow: 0 6px 20px rgba(120, 186, 126, 0.5);
}

.consult-checkbox {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  cursor: pointer;
  font-size: 0.9rem;
  color: white;
}

.consult-checkbox input[type="checkbox"] {
  width: auto;
  margin: 0;
}

.selected-services {
  margin-bottom: 1.5rem;
}

.selected-services h4 {
  font-size: 1.1rem;
  color: white;
  margin-bottom: 1rem;
}

.selected-services ul {
  list-style: none;
  padding: 0;
}

.selected-service-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: rgba(255, 255, 255, 0.1);
  padding: 0.8rem 1rem;
  border-radius: 10px;
  margin-bottom: 0.5rem;
  color: white;
  gap: 1rem;
}

.service-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.service-name {
  font-weight: 600;
  font-size: 0.95rem;
}

.service-price {
  font-size: 0.85rem;
  opacity: 0.8;
}

.quantity-control {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  flex-shrink: 0;
}

.quantity-control label {
  font-size: 0.8rem;
  margin: 0;
}

.quantity-input {
  width: 50px;
  padding: 0.3rem;
  border-radius: 5px;
  text-align: center;
  font-size: 0.9rem;
}

.remove-service-btn {
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 50%;
  width: 28px;
  height: 28px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 1.1rem;
  transition: all 0.3s ease;
  flex-shrink: 0;
}

.remove-service-btn:hover {
  background: #dc2626;
  transform: scale(1.1);
}

.submit-btn {
  background: linear-gradient(45deg, #f59e0b, #f97316);
  color: white;
  padding: 1.1rem 2.5rem;
  border: none;
  border-radius: 50px;
  font-size: 1.1rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
  width: 100%;
  margin-top: 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.submit-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 10px 25px rgba(245, 158, 11, 0.4);
}

.submit-btn:disabled {
  background: #9ca3af;
  cursor: not-allowed;
}

.btn-icon {
  font-size: 1.2rem;
}

/* About Section */
.about-section {
  background: linear-gradient(135deg, #029660 5%, #6CA374 95%);
  color: #fff;
}

.about-list li {
  margin-bottom: 10px;
  font-size: 1.1rem;
}

.about-image-box {
  background-color: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(6px);
  display: inline-block;
}

/* Error & Loading */
.error {
  text-align: center;
  padding: 2rem;
  color: #ef4444;
  font-size: 1.1rem;
}

.loading {
  text-align: center;
  padding: 2rem;
  color: #6b7280;
}

.loading-spinner {
  border: 4px solid #f3f4f6;
  border-top: 4px solid #78ba7e;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  animation: spin 1s linear infinite;
  margin: 0 auto 1rem;
}

/* Animations */
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

/* Mobile Responsive Styles */
@media (max-width: 1200px) {
  .container {
    max-width: 100%;
  }
  
  .services-grid {
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  }
}

@media (max-width: 992px) {
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

@media (max-width: 768px) {
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
}

/* Mobile Responsive Styles */
@media (max-width: 576px) {
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
    height: 90px; /* Giảm 25% từ 120px */
  }

  .service-content {
    padding: 0.8rem;
    display: flex;
    flex-direction: column;
    justify-content: space-between; /* Đảm bảo nút nằm dưới cùng */
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
    margin-top: 0; /* Loại bỏ margin-top để flex tự điều chỉnh */
    align-self: center; /* Căn giữa nút theo chiều ngang */
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
}
</style>