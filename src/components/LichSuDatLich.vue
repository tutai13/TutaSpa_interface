<template>
  <div class="spa-history-container">
    <!-- Header Section -->
    <div class="header-section">
      <h1 class="page-title">Lịch Sử Đặt Lịch</h1>
      <p class="page-subtitle">
        Theo dõi các lần đặt lịch và trải nghiệm dịch vụ của bạn
      </p>
    </div>

    <!-- Empty State -->
    <div v-if="lichSu.length === 0" class="empty-state">
      <div class="empty-icon">🌿</div>
      <h3>Chưa có lịch sử đặt lịch</h3>
      <p>Hãy trải nghiệm những dịch vụ tuyệt vời của chúng tôi</p>
    </div>

    <!-- History Cards -->
    <div class="history-grid">
      <div
        v-for="datLich in lichSu"
        :key="datLich.datLichID"
        class="history-card"
      >
        <!-- Card Header -->
        <div class="card-header">
          <div class="date-info">
            <div class="date-icon">📅</div>
            <div>
              <div class="date-text">{{ formatDate(datLich.thoiGian) }}</div>
              <div class="payment-method">
                {{ datLich.daThanhToan ? "Đã thanh toán" : "Chưa thanh toán" }}
              </div>
            </div>
          </div>
          <div class="status-badge">{{ datLich.trangThai }}</div>
        </div>

        <!-- Services List -->
        <div class="services-list">
          <div
            v-for="ct in datLich.chiTietDatLichs"
            :key="ct.chiTietDatLichID"
            class="service-item"
          >
            <div class="service-main">
              <div class="service-icon">🌸</div>
              <div class="service-details">
                <h4 class="service-name">{{ ct.dichVu.tenDichVu }}</h4>
                <div class="service-info">
                  <span class="quantity">Số lượng: {{ ct.soLuongDV }}</span>
                  <span class="unit-price"
                    >Giá: {{ formatCurrency(ct.dichVu.gia) }}</span
                  >
                </div>
              </div>
              <div class="service-total">
                {{ formatCurrency(ct.dichVu.gia * ct.soLuongDV) }}
              </div>
            </div>

            <!-- Action Button -->
            <div class="service-actions">
              <router-link
                v-if="datLich.daThanhToan && ct.dichVu"
                :to="`/DanhGia/${ct.dichVu.dichVuID}`"
                class="review-btn"
              >
                <i class="fa-regular fa-star"></i>
                Đánh giá dịch vụ
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import apiClient from "../utils/axiosClient";

const lichSu = ref([]);

onMounted(() => {
  loadLichSu();
});

const loadLichSu = async () => {
  try {
    const res = await apiClient.get("/DatLich/by-user");
    lichSu.value = res;
  } catch (err) {
    console.error("❌ Lỗi tải lịch sử dịch vụ:", err);
  }
};

const formatDate = (str) => {
  const d = new Date(str);
  return d.toLocaleDateString("vi-VN", {
    year: "numeric",
    month: "2-digit",
    day: "2-digit",
    hour: "2-digit",
    minute: "2-digit",
  });
};

const formatCurrency = (value) =>
  new Intl.NumberFormat("vi-VN", {
    style: "currency",
    currency: "VND",
  }).format(value);
// --- Safe helpers to handle items that may be service OR product (or missing) ---
const getTen = (ct) => {
  if (ct && ct.dichVu) return ct.dichVu.tenDichVu;
  if (ct && ct.sanPham)
    return ct.sanPham.tenSanPham || `Sản phẩm #${ct.sanPhamID ?? ""}`;
  // Fallback if both null
  const label = ct?.dichVuID
    ? `Dịch vụ #${ct.dichVuID}`
    : ct?.sanPhamID
    ? `Sản phẩm #${ct.sanPhamID}`
    : "Mục không xác định";
  return label;
};

const getGia = (ct) => {
  if (ct && ct.dichVu) return ct.dichVu.gia;
  if (ct && ct.sanPham) {
    // Prefer explicit price on product if available, otherwise derive from line total
    return (
      ct.sanPham.gia ??
      ((ct?.soLuongSP
        ? Number(ct.thanhTien) / Number(ct.soLuongSP)
        : Number(ct.thanhTien)) ||
        0)
    );
  }
  // If neither, try to derive unit price from line
  return (
    (ct?.soLuongSP
      ? Number(ct?.thanhTien) / Number(ct?.soLuongSP)
      : Number(ct?.thanhTien)) || 0
  );
};
// ------------------------------------------------------------------------------
</script>

<style scoped>
.spa-history-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #f5f9f5 0%, #e8f5e8 100%);
  padding: 2rem 1rem;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

/* Header Section */
.header-section {
  text-align: center;
  margin-bottom: 3rem;
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
}

.page-title {
  font-size: 2.5rem;
  font-weight: 700;
  color: #2d5a2d;
  margin-bottom: 1rem;
  letter-spacing: -0.02em;
}

.page-subtitle {
  font-size: 1.1rem;
  color: #5a7a5a;
  margin-bottom: 0;
  line-height: 1.6;
}

/* Empty State */
.empty-state {
  text-align: center;
  padding: 4rem 2rem;
  background: white;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(45, 90, 45, 0.1);
  max-width: 500px;
  margin: 0 auto;
}

.empty-icon {
  font-size: 4rem;
  margin-bottom: 1.5rem;
}

.empty-state h3 {
  color: #2d5a2d;
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
}

.empty-state p {
  color: #5a7a5a;
  font-size: 1rem;
}

/* History Grid */
.history-grid {
  max-width: 1200px;
  margin: 0 auto;
  display: grid;
  gap: 2rem;
}

.history-card {
  background: white;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(45, 90, 45, 0.1);
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.history-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 20px 40px rgba(45, 90, 45, 0.15);
}

/* Card Header */
.card-header {
  background: linear-gradient(135deg, #6ba86b 0%, #4a8c4a 100%);
  color: white;
  padding: 1.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.date-info {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.date-icon {
  font-size: 2rem;
  opacity: 0.9;
}

.date-text {
  font-size: 1.1rem;
  font-weight: 600;
  margin-bottom: 0.25rem;
}

.payment-method {
  font-size: 0.9rem;
  opacity: 0.9;
}

.status-badge {
  background: rgba(255, 255, 255, 0.2);
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-size: 0.9rem;
  font-weight: 500;
  backdrop-filter: blur(10px);
}

/* Services List */
.services-list {
  padding: 0;
}

.service-item {
  padding: 1.5rem;
  border-bottom: 1px solid #f0f0f0;
}

.service-item:last-child {
  border-bottom: none;
}

.service-main {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  margin-bottom: 1rem;
}

.service-icon {
  font-size: 2rem;
  flex-shrink: 0;
}

.service-details {
  flex: 1;
}

.service-name {
  color: #2d5a2d;
  font-size: 1.2rem;
  font-weight: 600;
  margin: 0 0 0.5rem 0;
  line-height: 1.4;
}

.service-info {
  display: flex;
  gap: 1.5rem;
  flex-wrap: wrap;
}

.quantity,
.unit-price {
  color: #5a7a5a;
  font-size: 0.95rem;
}

.service-total {
  color: #2d5a2d;
  font-size: 1.3rem;
  font-weight: 700;
  text-align: right;
  flex-shrink: 0;
}

/* Service Actions */
.service-actions {
  display: flex;
  justify-content: flex-end;
}

.review-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  background: linear-gradient(135deg, #6ba86b 0%, #4a8c4a 100%);
  color: white;
  padding: 0.75rem 1.5rem;
  border-radius: 25px;
  text-decoration: none;
  font-weight: 500;
  font-size: 0.95rem;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(107, 168, 107, 0.3);
}

.review-btn:hover {
  background: linear-gradient(135deg, #4a8c4a 0%, #3a7a3a 100%);
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(107, 168, 107, 0.4);
  color: white;
}

.review-btn i {
  font-size: 1rem;
}

/* Responsive Design */
@media (max-width: 768px) {
  .spa-history-container {
    padding: 1rem 0.5rem;
  }

  .page-title {
    font-size: 2rem;
  }

  .card-header {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }

  .service-main {
    flex-direction: column;
    gap: 1rem;
  }

  .service-total {
    text-align: left;
    font-size: 1.2rem;
  }

  .service-info {
    flex-direction: column;
    gap: 0.5rem;
  }

  .service-actions {
    justify-content: stretch;
  }

  .review-btn {
    width: 100%;
    justify-content: center;
  }
}

@media (max-width: 480px) {
  .page-title {
    font-size: 1.7rem;
  }

  .service-icon {
    font-size: 1.5rem;
  }

  .service-name {
    font-size: 1.1rem;
  }
}
</style>
