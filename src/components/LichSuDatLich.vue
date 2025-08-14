<template>
  <section class="history-section">
    <div class="container">
      <h2 class="section-title font-lora">
        🧾 Lịch Sử Sử Dụng Dịch Vụ
      </h2>
      <p class="section-subtitle">
        Xem lại các dịch vụ bạn đã trải nghiệm tại TutaSpa và để lại đánh giá cho chúng tôi nhé!
      </p>

      <div v-if="lichSu.length === 0" class="history-empty">
        <span class="empty-icon">📭</span>
        <div>Bạn chưa sử dụng dịch vụ nào.</div>
      </div>

      <div class="history-grid">
        <div
          v-for="hoaDon in lichSu"
          :key="hoaDon.hoaDonID"
          class="history-card"
        >
          <div class="history-card-header">
            <span class="history-date">🗓️ {{ formatDate(hoaDon.ngayTao) }}</span>
            <span class="history-pay">💳 {{ hoaDon.hinhThucThanhToan }}</span>
          </div>
          <div class="history-card-body">
            <ul class="history-list">
              <li
                v-for="ct in hoaDon.chiTietHoaDons"
                :key="ct.chiTietHoaDonID"
                class="history-list-item"
              >
                <div class="service-info">
                  <strong class="service-name">{{ ct.dichVu.tenDichVu }}</strong>
                  <span class="service-qty">Số lượng: {{ ct.soLuongSP }}</span>
                  <span class="service-price">Giá: {{ formatCurrency(ct.dichVu.gia) }}</span>
                  <span class="service-total">Thành tiền: {{ formatCurrency(ct.thanhTien) }}</span>
                </div>
                <router-link
                  :to="`/DanhGia/${ct.dichVu.dichVuID}`"
                  class="btn-rate"
                >
                  <i class="fa-regular fa-calendar-check"></i> Đánh Giá
                </router-link>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </section>
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
    const res = await apiClient.get("/ThanhToan/by-user");
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
</script>

<style scoped>
.history-section {
  padding: 8rem 2rem;
  background: linear-gradient(135deg, #f8fdf8 0%, #f0fdf4 50%, #e8f5e8 100%);
  min-height: 100vh;
}
.section-title {
  text-align: center;
  font-size: 2.8rem;
  margin-bottom: 1rem;
  color: #2d4a2d;
  font-family: "Lora", serif;
  position: relative;
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
.section-subtitle {
  text-align: center;
  font-size: 1.1rem;
  color: #4b5563;
  margin-bottom: 4rem;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}
.history-empty {
  text-align: center;
  color: #6b7280;
  font-size: 1.2rem;
  margin-top: 4rem;
  margin-bottom: 6rem;
}
.empty-icon {
  font-size: 3rem;
  display: block;
  margin-bottom: 1rem;
}
.history-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(340px, 1fr));
  gap: 2.5rem;
}
.history-card {
  background: white;
  border-radius: 20px;
  box-shadow: 0 8px 25px rgba(120, 186, 126, 0.12);
  border: 1px solid #e8f5e8;
  padding: 2rem 2rem 1.5rem 2rem;
  transition: all 0.4s ease;
  position: relative;
}
.history-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 20px 40px rgba(120, 186, 126, 0.2);
}
.history-card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #f0fdf4;
  border-radius: 12px;
  padding: 0.8rem 1rem;
  font-weight: 500;
  font-size: 1.1rem;
  color: #2d4a2d;
  margin-bottom: 1.2rem;
}
.history-list {
  list-style: none;
  padding: 0;
  margin: 0;
}
.history-list-item {
  background: rgba(120, 186, 126, 0.07);
  border-radius: 12px;
  padding: 1rem 1rem 1rem 1.2rem;
  margin-bottom: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1.5rem;
}
.service-info {
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}
.service-name {
  color: #2d4a2d;
  font-size: 1.1rem;
  font-family: "Lora", serif;
}
.service-qty,
.service-price,
.service-total {
  color: #4b5563;
  font-size: 0.98rem;
}
.btn-rate {
  background: linear-gradient(45deg, #78ba7e, #5e8c64);
  color: white;
  padding: 0.6rem 1.5rem;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.95rem;
  text-decoration: none;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  transition: all 0.3s ease;
}
.btn-rate:hover {
  transform: scale(1.05);
  box-shadow: 0 6px 20px rgba(120, 186, 126, 0.2);
  background: linear-gradient(45deg, #5e8c64, #78ba7e);
}
@media (max-width: 768px) {
  .history-grid {
    grid-template-columns: 1fr;
  }
  .section-title {
    font-size: 2.2rem;
  }
}
.font-lora {
  font-family: "Lora", serif;
}
</style>
