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

    <!-- Two Column Layout -->
    <div v-else class="history-layout">
      <!-- Left Column - Đã thanh toán -->
      <div class="history-column left-column">
        <div class="column-header completed">
          <div class="header-content">
            <div class="header-icon">✨</div>
            <div>
              <h2 class="column-title">Lịch Đã Hoàn Thành</h2>
              <p class="column-subtitle">{{ lichSuDaThanhToan.length }} lịch hẹn</p>
            </div>
          </div>
        </div>
        
        <div class="history-cards">
          <div
            v-for="datLich in lichSuDaThanhToan"
            :key="datLich.datLichID"
            class="history-card completed-card"
          >
            <!-- Card Header -->
            <div class="card-header completed-header">
              <div class="date-info">
                <div class="date-icon">📅</div>
                <div>
                  <div class="date-text">{{ formatDate(datLich.thoiGian) }}</div>
                  <div class="payment-status completed">✅ Đã thanh toán</div>
                </div>
              </div>
              <div class="status-badge completed-badge">{{ datLich.trangThai }}</div>
            </div>

            <!-- Services List -->
            <div class="services-list">
              <div
                v-for="ct in datLich.chiTietDatLichs"
                :key="ct.chiTietDatLichID"
                class="service-item"
              >
                <div class="service-main">
                  <div class="service-icon completed-icon">🌸</div>
                  <div class="service-details">
                    <h4 class="service-name">{{ ct.dichVu.tenDichVu }}</h4>
                    <div class="service-info">
                      <span class="quantity">Số lượng: {{ ct.soLuongDV }}</span>
                      <span class="unit-price">Giá: {{ formatCurrency(ct.dichVu.gia) }}</span>
                    </div>
                  </div>
                  <div class="service-total completed-total">
                    {{ formatCurrency(ct.dichVu.gia * ct.soLuongDV) }}
                  </div>
                </div>

                <!-- Action Button -->
                <div class="service-actions">
                  <router-link
                    v-if="ct.dichVu"
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

      <!-- Right Column - Chưa thanh toán -->
      <div class="history-column right-column">
        <div class="column-header pending">
          <div class="header-content">
            <div class="header-icon">⏳</div>
            <div>
              <h2 class="column-title">Lịch Đã Đặt</h2>
              <p class="column-subtitle">{{ lichSuChuaThanhToan.length }} lịch hẹn</p>
            </div>
          </div>
        </div>
        
        <div class="history-cards">
          <div
            v-for="datLich in lichSuChuaThanhToan"
            :key="datLich.datLichID"
            class="history-card pending-card"
          >
            <!-- Card Header -->
            <div class="card-header pending-header">
              <div class="date-info">
                <div class="date-icon">📅</div>
                <div>
                  <div class="date-text">{{ formatDate(datLich.thoiGian) }}</div>
                  <div class="payment-status pending">⏳ Chưa thanh toán</div>
                </div>
              </div>
              <div class="status-badge pending-badge">{{ datLich.trangThai }}</div>
            </div>

            <!-- Services List -->
            <div class="services-list">
              <div
                v-for="ct in datLich.chiTietDatLichs"
                :key="ct.chiTietDatLichID"
                class="service-item"
              >
                <div class="service-main">
                  <div class="service-icon pending-icon">🌿</div>
                  <div class="service-details">
                    <h4 class="service-name">{{ ct.dichVu.tenDichVu }}</h4>
                    <div class="service-info">
                      <span class="quantity">Số lượng: {{ ct.soLuongDV }}</span>
                      <span class="unit-price">Giá: {{ formatCurrency(ct.dichVu.gia) }}</span>
                    </div>
                  </div>
                  <div class="service-total pending-total">
                    {{ formatCurrency(ct.dichVu.gia * ct.soLuongDV) }}
                  </div>
                </div>

                <!-- New Order Badge -->
                <div class="new-order-badge">
                  <span class="new-badge">MỚI</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from "vue";
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

// Computed properties for sorting and filtering
const lichSuDaThanhToan = computed(() => {
  return lichSu.value
    .filter(item => item.daThanhToan)
    .sort((a, b) => {
      // Sắp xếp theo thời gian mới nhất và ưu tiên những lịch chưa đánh giá
      const timeA = new Date(a.thoiGian).getTime();
      const timeB = new Date(b.thoiGian).getTime();
      return timeB - timeA; // Mới nhất lên trước
    });
});

const lichSuChuaThanhToan = computed(() => {
  return lichSu.value
    .filter(item => !item.daThanhToan)
    .sort((a, b) => {
      // Sắp xếp theo thời gian mới nhất lên trước
      const timeA = new Date(a.thoiGian).getTime();
      const timeB = new Date(b.thoiGian).getTime();
      return timeB - timeA; // Mới nhất lên trước
    });
});

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

// Safe helpers to handle items that may be service OR product (or missing)
const getTen = (ct) => {
  if (ct && ct.dichVu) return ct.dichVu.tenDichVu;
  if (ct && ct.sanPham)
    return ct.sanPham.tenSanPham || `Sản phẩm #${ct.sanPhamID ?? ""}`;
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
    return (
      ct.sanPham.gia ??
      ((ct?.soLuongSP
        ? Number(ct.thanhTien) / Number(ct.soLuongSP)
        : Number(ct.thanhTien)) ||
        0)
    );
  }
  return (
    (ct?.soLuongSP
      ? Number(ct?.thanhTien) / Number(ct?.soLuongSP)
      : Number(ct?.thanhTien)) || 0
  );
};
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
  max-width: 1200px;
  margin-left: auto;
  margin-right: auto;
}

.page-title {
  font-size: 2.8rem;
  font-weight: 700;
  color: #2d5a2d;
  margin-bottom: 1rem;
  letter-spacing: -0.02em;
  text-shadow: 0 2px 4px rgba(45, 90, 45, 0.1);
}

.page-subtitle {
  font-size: 1.2rem;
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

/* Two Column Layout */
.history-layout {
  max-width: 1400px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  align-items: start;
}

.history-column {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

/* Column Headers */
.column-header {
  background: white;
  border-radius: 15px;
  padding: 1.5rem;
  box-shadow: 0 8px 25px rgba(45, 90, 45, 0.1);
  border-top: 4px solid;
  position: sticky;
  top: 1rem;
  z-index: 10;
}

.column-header.completed {
  border-top-color: #6ba86b;
  background: linear-gradient(135deg, rgba(107, 168, 107, 0.05) 0%, rgba(107, 168, 107, 0.02) 100%);
}

.column-header.pending {
  border-top-color: #e67e22;
  background: linear-gradient(135deg, rgba(230, 126, 34, 0.05) 0%, rgba(230, 126, 34, 0.02) 100%);
}

.header-content {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.header-icon {
  font-size: 2.5rem;
}

.column-title {
  color: #2d5a2d;
  font-size: 1.4rem;
  font-weight: 700;
  margin: 0 0 0.25rem 0;
}

.column-subtitle {
  color: #5a7a5a;
  font-size: 0.95rem;
  margin: 0;
  opacity: 0.8;
}

/* History Cards */
.history-cards {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.history-card {
  background: white;
  border-radius: 18px;
  box-shadow: 0 8px 25px rgba(45, 90, 45, 0.08);
  overflow: hidden;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
}

.history-card:hover {
  transform: translateY(-5px) scale(1.02);
  box-shadow: 0 15px 35px rgba(45, 90, 45, 0.15);
}

/* Completed Cards */
.completed-card {
  border-left: 4px solid #6ba86b;
}

.completed-card::before {
  content: '';
  position: absolute;
  top: 0;
  right: 0;
  width: 50px;
  height: 50px;
  background: linear-gradient(135deg, #6ba86b, #4a8c4a);
  clip-path: polygon(100% 0%, 0% 0%, 100% 100%);
}

/* Pending Cards */
.pending-card {
  border-left: 4px solid #e67e22;
  animation: pulse-new 2s infinite;
}

.pending-card::before {
  content: '';
  position: absolute;
  top: 0;
  right: 0;
  width: 50px;
  height: 50px;
  background: linear-gradient(135deg, #e67e22, #d35400);
  clip-path: polygon(100% 0%, 0% 0%, 100% 100%);
}

@keyframes pulse-new {
  0%, 100% { box-shadow: 0 8px 25px rgba(45, 90, 45, 0.08); }
  50% { box-shadow: 0 8px 25px rgba(230, 126, 34, 0.2); }
}

/* Card Headers */
.card-header {
  color: white;
  padding: 1.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: relative;
  z-index: 2;
}

.completed-header {
  background: linear-gradient(135deg, #6ba86b 0%, #4a8c4a 100%);
}

.pending-header {
  background: linear-gradient(135deg, #e67e22 0%, #d35400 100%);
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

.payment-status {
  font-size: 0.85rem;
  padding: 0.25rem 0.75rem;
  border-radius: 12px;
  font-weight: 500;
}

.payment-status.completed {
  background: rgba(255, 255, 255, 0.2);
  color: #fff;
}

.payment-status.pending {
  background: rgba(255, 255, 255, 0.2);
  color: #fff;
}

.status-badge {
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-size: 0.85rem;
  font-weight: 500;
  backdrop-filter: blur(10px);
}

.completed-badge {
  background: rgba(255, 255, 255, 0.25);
}

.pending-badge {
  background: rgba(255, 255, 255, 0.25);
}

/* Services List */
.services-list {
  padding: 0;
}

.service-item {
  padding: 1.5rem;
  border-bottom: 1px solid #f5f5f5;
  position: relative;
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
  font-size: 2.2rem;
  flex-shrink: 0;
}

.completed-icon {
  filter: hue-rotate(0deg);
}

.pending-icon {
  filter: hue-rotate(30deg);
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
  font-size: 1.3rem;
  font-weight: 700;
  text-align: right;
  flex-shrink: 0;
}

.completed-total {
  color: #6ba86b;
}

.pending-total {
  color: #e67e22;
}

/* New Order Badge */
.new-order-badge {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 1rem;
}

.new-badge {
  background: linear-gradient(135deg, #e67e22, #d35400);
  color: white;
  padding: 0.4rem 1rem;
  border-radius: 15px;
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 0.5px;
  animation: glow-new 1.5s ease-in-out infinite alternate;
}

@keyframes glow-new {
  from { box-shadow: 0 0 5px rgba(230, 126, 34, 0.5); }
  to { box-shadow: 0 0 15px rgba(230, 126, 34, 0.8); }
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
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 4px 15px rgba(107, 168, 107, 0.3);
}

.review-btn:hover {
  background: linear-gradient(135deg, #4a8c4a 0%, #3a7a3a 100%);
  transform: translateY(-2px) scale(1.05);
  box-shadow: 0 8px 25px rgba(107, 168, 107, 0.4);
  color: white;
}

.review-btn i {
  font-size: 1rem;
}

/* Responsive Design */
@media (max-width: 1024px) {
  .history-layout {
    grid-template-columns: 1fr;
    gap: 2rem;
  }
  
  .column-header {
    position: static;
  }
}

@media (max-width: 768px) {
  .spa-history-container {
    padding: 1rem 0.5rem;
  }

  .page-title {
    font-size: 2.2rem;
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
  
  .column-title {
    font-size: 1.2rem;
  }
}

@media (max-width: 480px) {
  .page-title {
    font-size: 1.8rem;
  }

  .service-icon {
    font-size: 1.8rem;
  }

  .service-name {
    font-size: 1.1rem;
  }
  
  .header-content {
    flex-direction: column;
    text-align: center;
    gap: 0.5rem;
  }
}
</style>