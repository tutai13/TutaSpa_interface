<template>
  <div class="register-container">
    <div class="register-header">
      <h1>Đăng ký</h1>
      <p>Tạo tài khoản mới để bắt đầu!</p>
    </div>

    <form @submit.prevent="handleRegister">
      <div class="form-group">
        <label for="name">Họ và tên</label>
        <div class="input-wrapper">
          <i class="fas fa-user input-icon"></i>
          <input 
            type="text" 
            id="name"
            class="form-control"
            v-model="form.name"
            placeholder="Nhập họ và tên"
            :class="{ 'error': errors.name }"
          >
        </div>
        <transition name="fade">
          <div v-if="errors.name" class="error-message">
            <i class="fas fa-exclamation-circle"></i>
            {{ errors.name }}
          </div>
        </transition>
      </div>

      <div class="form-group">
        <label for="phone">Số điện thoại</label>
        <div class="input-wrapper">
          <i class="fas fa-phone input-icon"></i>
          <input 
            type="tel" 
            id="phone"
            class="form-control"
            v-model="form.phone"
            placeholder="Nhập số điện thoại"
            :class="{ 'error': errors.phone }"
            :disabled="otpSent"
          >
        </div>
        <transition name="fade">
          <div v-if="errors.phone" class="error-message">
            <i class="fas fa-exclamation-circle"></i>
            {{ errors.phone }}
          </div>
        </transition>
      </div>

      <div class="form-group">
        <div class="otp-section">
          <button 
            type="button"
            class="send-otp-btn"
            @click="sendOTP"
            :disabled="isLoadingOTP || !form.phone || otpSent"
          >
            <span v-if="isLoadingOTP">
              <i class="fas fa-spinner fa-spin"></i> Đang gửi...
            </span>
            <span v-else-if="otpSent">
              <i class="fas fa-check"></i> Đã gửi OTP
            </span>
            <span v-else>
              <i class="fas fa-paper-plane"></i> Gửi mã OTP
            </span>
          </button>

          <div class="otp-input-section" v-if="otpSent">
            <label for="otp">Mã OTP</label>
            <div class="input-wrapper">
              <i class="fas fa-shield-alt input-icon"></i>
              <input 
                type="text" 
                id="otp"
                class="form-control"
                v-model="form.otp"
                placeholder="Nhập mã OTP (6 số)"
                :class="{ 'error': errors.otp, 'verified': otpVerified }"
                maxlength="6"
                :disabled="otpVerified"
              >
              <button 
                type="button"
                class="resend-btn"
                @click="resendOTP"
                :disabled="countdown > 0 || otpVerified"
                v-if="!otpVerified"
              >
                <span v-if="countdown > 0">{{ countdown }}s</span>
                <span v-else>Gửi lại</span>
              </button>
              <button 
                type="button"
                class="verify-otp-btn"
                @click="verifyOTP"
                :disabled="isVerifyingOTP || !form.otp || otpVerified"
                v-if="!otpVerified"
              >
                <span v-if="isVerifyingOTP">
                  <i class="fas fa-spinner fa-spin"></i>
                </span>
                <span v-else>
                  <i class="fas fa-check"></i> Xác nhận
                </span>
              </button>
              <div class="verified-badge" v-if="otpVerified">
                <i class="fas fa-check-circle"></i> Đã xác nhận
              </div>
            </div>
            <transition name="fade">
              <div v-if="errors.otp" class="error-message">
                <i class="fas fa-exclamation-circle"></i>
                {{ errors.otp }}
              </div>
            </transition>
          </div>
        </div>
      </div>

      <div class="form-group">
        <label for="password">Mật khẩu</label>
        <div class="input-wrapper">
          <i class="fas fa-lock input-icon"></i>
          <input 
            :type="showPassword ? 'text' : 'password'"
            id="password"
            class="form-control"
            v-model="form.password"
            placeholder="Nhập mật khẩu"
            :class="{ 'error': errors.password }"
          >
          <i 
            :class="showPassword ? 'fas fa-eye-slash' : 'fas fa-eye'"
            class="password-toggle"
            @click="togglePassword"
          ></i>
        </div>
        <transition name="fade">
          <div v-if="errors.password" class="error-message">
            <i class="fas fa-exclamation-circle"></i>
            {{ errors.password }}
          </div>
        </transition>
      </div>

      <div class="form-group">
        <label for="confirmPassword">Xác nhận mật khẩu</label>
        <div class="input-wrapper">
          <i class="fas fa-lock input-icon"></i>
          <input 
            :type="showConfirmPassword ? 'text' : 'password'"
            id="confirmPassword"
            class="form-control"
            v-model="form.confirmPassword"
            placeholder="Nhập lại mật khẩu"
            :class="{ 'error': errors.confirmPassword }"
          >
          <i 
            :class="showConfirmPassword ? 'fas fa-eye-slash' : 'fas fa-eye'"
            class="password-toggle"
            @click="toggleConfirmPassword"
          ></i>
        </div>
        <transition name="fade">
          <div v-if="errors.confirmPassword" class="error-message">
            <i class="fas fa-exclamation-circle"></i>
            {{ errors.confirmPassword }}
          </div>
        </transition>
      </div>

      <button 
        type="submit" 
        class="register-btn"
        :disabled="isLoading || !otpVerified"
      >
        <span v-if="isLoading">
          <i class="fas fa-spinner fa-spin"></i> Đang đăng ký...
        </span>
        <span v-else>
          <i class="fas fa-user-plus"></i> Đăng ký
        </span>
      </button>

      <div class="login-link">
        <p>Đã có tài khoản?</p>
        <a href="#" @click.prevent="goToLogin">Đăng nhập ngay</a>
      </div>
    </form>
  </div>
</template>

<script setup>
import notification from '../notification'
import { ref, reactive, onUnmounted } from 'vue'
import { useRouter } from 'vue-router'
import { authAPI } from '../services/authservice' // Import API service

// Router instance
const router = useRouter()

// Reactive data
const form = reactive({
  name: '',
  phone: '',
  otp: '',
  password: '',
  confirmPassword: ''
})

const errors = reactive({
  name: '',
  phone: '',
  otp: '',
  password: '',
  confirmPassword: ''
})

// Refs
const showPassword = ref(false)
const showConfirmPassword = ref(false)
const isLoading = ref(false)
const isLoadingOTP = ref(false)
const isVerifyingOTP = ref(false)
const otpSent = ref(false)
const otpVerified = ref(false)
const countdown = ref(0)
let countdownInterval = null

// Methods
const validateForm = () => {
  let isValid = true
  
  // Reset errors
  errors.name = ''
  errors.phone = ''
  errors.password = ''
  errors.confirmPassword = ''

  // Validate name
  if (!form.name.trim()) {
    errors.name = 'Vui lòng nhập họ và tên'
    isValid = false
  } else if (form.name.trim().length < 2) {
    errors.name = 'Họ và tên phải có ít nhất 2 ký tự'
    isValid = false
  }

  // Validate phone
  if (!form.phone.trim()) {
    errors.phone = 'Vui lòng nhập số điện thoại'
    isValid = false
  } else if (!/^[0-9]{10,11}$/.test(form.phone.replace(/\s/g, ''))) {
    errors.phone = 'Số điện thoại không hợp lệ'
    isValid = false
  }

  // Validate password
  if (!form.password.trim()) {
    errors.password = 'Vui lòng nhập mật khẩu'
    isValid = false
  } else if (form.password.length < 6) {
    errors.password = 'Mật khẩu phải có ít nhất 6 ký tự'
    isValid = false
  }

  // Validate confirm password
  if (!form.confirmPassword.trim()) {
    errors.confirmPassword = 'Vui lòng xác nhận mật khẩu'
    isValid = false
  } else if (form.password !== form.confirmPassword) {
    errors.confirmPassword = 'Mật khẩu xác nhận không khớp'
    isValid = false
  }

  return isValid
}

const sendOTP = async () => {
  // Validate phone first
  if (!form.phone.trim()) {
    errors.phone = 'Vui lòng nhập số điện thoại'
    return
  }
  
  if (!/^[0-9]{10,11}$/.test(form.phone.replace(/\s/g, ''))) {
    errors.phone = 'Số điện thoại không hợp lệ'
    return
  }

  isLoadingOTP.value = true
  errors.phone = ''

  try {
    // Gọi API gửi OTP
    const result = await authAPI.sendOTP(form.phone)

    if (result.code == 106 || result.code == 203) {
      otpSent.value = true
      otpVerified.value = false // Reset verification status
      startCountdown()
      notification.success("Mã OTP đã được gửi đến số điện thoại của bạn", {
        title: "",
        timer: 2000
      })
    } else {
      notification.error("Gửi OTP thất bại")
    }
  } catch (error) {
    console.error('Send OTP error:', error)
    notification.error(error.message ? error.message : 'Gửi OTP thất bại. Vui lòng thử lại!')
  } finally {
    isLoadingOTP.value = false
  }
}

const verifyOTP = async () => {
  // Validate OTP first
  if (!form.otp.trim()) {
    errors.otp = 'Vui lòng nhập mã OTP'
    return
  }
  
  if (!/^[0-9]{6}$/.test(form.otp)) {
    errors.otp = 'Mã OTP phải có 6 số'
    return
  }

  isVerifyingOTP.value = true
  errors.otp = ''

  try {
    // Gọi API xác thực OTP
    const result = await authAPI.verifyOTP(form.phone, form.otp)

    if (result) {
      otpVerified.value = true
      notification.success("Xác thực OTP thành công!", {
        title: "",
        timer: 2000
      })
    } else {
      errors.otp = 'Mã OTP không chính xác'
      notification.error("Mã OTP không chính xác")
    }
  } catch (error) {
    console.error('Verify OTP error:', error)
    errors.otp = error.message || 'Xác thực OTP thất bại'
    notification.error(error.message ? error.message : 'Xác thực OTP thất bại. Vui lòng thử lại!')
  } finally {
    isVerifyingOTP.value = false
  }
}

const resendOTP = async () => {
  otpVerified.value = false // Reset verification status when resending
  form.otp = '' // Clear OTP input
  await sendOTP()
}

const startCountdown = () => {
  countdown.value = 60
  countdownInterval = setInterval(() => {
    countdown.value--
    if (countdown.value <= 0) {
      clearInterval(countdownInterval)
    }
  }, 1000)
}

const handleRegister = async () => {
  if (!validateForm()) return

  if (!otpVerified.value) {
    notification.error("Vui lòng xác thực OTP trước khi đăng ký")
    return
  }

  isLoading.value = true

  try {
    // Gọi API đăng ký thực tế
    const result = await authAPI.register({
      Name: form.name,
      PhoneNumber: form.phone,
      Password: form.password,
      ConfirmPassword: form.confirmPassword
    })

    if (result) {
      notification.success("Đăng ký thành công! Vui lòng đăng nhập.", {
        title: "",
        timer: 2000
      })
      
      await router.push('/login')
      return
    }

    notification.error("Đăng ký thất bại")
    
  } catch (error) {
    console.error('Register error:', error)
    notification.error(error.message ? error.message : 'Đăng ký thất bại. Vui lòng thử lại!')
  } finally {
    isLoading.value = false
  }
}

const togglePassword = () => {
  showPassword.value = !showPassword.value
}

const toggleConfirmPassword = () => {
  showConfirmPassword.value = !showConfirmPassword.value
}

const goToLogin = () => {
  router.push('/login')
}

// Cleanup countdown on unmount
onUnmounted(() => {
  if (countdownInterval) {
    clearInterval(countdownInterval)
  }
})

// Emit events (nếu cần thiết cho parent component)
const emit = defineEmits(['register-success', 'go-to-login'])
</script>

<style scoped>
.register-container {
  background: #f5faf5;
  padding: 40px;
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  border-radius: 15px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.register-header {
  text-align: center;
  margin-bottom: 30px;
}

.register-header h1 {
  color: #2e7d32;
  font-size: 32px;
  font-weight: 700;
  margin-bottom: 10px;
}

.register-header p {
  color: #666;
  font-size: 16px;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  color: #2e7d32;
  font-weight: 500;
  margin-bottom: 8px;
  font-size: 14px;
}

.input-wrapper {
  position: relative;
  background: #fff;
  border: 1px solid #ccc;
  border-radius: 8px;
  overflow: hidden;
}

.form-control {
  width: 100%;
  padding: 12px 40px;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  background: transparent;
}

.form-control:focus {
  outline: none;
  box-shadow: 0 0 5px rgba(46, 125, 50, 0.3);
}

.form-control.error {
  border-color: #d32f2f;
}

.form-control.verified {
  border-color: #2e7d32;
}

.input-icon {
  position: absolute;
  left: 12px;
  top: 50%;
  transform: translateY(-50%);
  color: #2e7d32;
  font-size: 16px;
}

.password-toggle {
  position: absolute;
  right: 12px;
  top: 50%;
  transform: translateY(-50%);
  color: #2e7d32;
  cursor: pointer;
  font-size: 16px;
}

.password-toggle:hover {
  color: #1b5e20;
}

.otp-section {
  margin-top: 15px;
}

.send-otp-btn {
  width: 100%;
  padding: 12px;
  background: #2e7d32;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.3s ease;
}

.send-otp-btn:hover {
  background: #1b5e20;
}

.send-otp-btn:disabled {
  background: #a5d6a7;
  cursor: not-allowed;
}

.otp-input-section {
  margin-top: 15px;
}

.otp-input-section label {
  display: block;
  color: #2e7d32;
  font-weight: 500;
  margin-bottom: 8px;
  font-size: 14px;
}

.resend-btn {
  position: absolute;
  right: 80px;
  top: 50%;
  transform: translateY(-50%);
  background: #757575;
  color: white;
  border: none;
  border-radius: 6px;
  padding: 6px 12px;
  font-size: 12px;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.3s ease;
}

.resend-btn:hover {
  background: #616161;
}

.verify-otp-btn {
  position: absolute;
  right: 8px;
  top: 50%;
  transform: translateY(-50%);
  background: #2e7d32;
  color: white;
  border: none;
  border-radius: 6px;
  padding: 6px 12px;
  font-size: 12px;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.3s ease;
}

.verify-otp-btn:hover {
  background: #1b5e20;
}

.verified-badge {
  position: absolute;
  right: 8px;
  top: 50%;
  transform: translateY(-50%);
  background: #2e7d32;
  color: white;
  border-radius: 6px;
  padding: 6px 12px;
  font-size: 12px;
  font-weight: 500;
  text-align: center;
}

.register-btn {
  width: 100%;
  padding: 14px;
  background: #ffca28;
  color: #fff;
  border: none;
  border-radius: 8px;
  font-size: 18px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.3s ease;
}

.register-btn:hover {
  background: #ffb300;
}

.register-btn:disabled {
  background: #ffecb3;
  cursor: not-allowed;
}

.login-link {
  text-align: center;
  padding-top: 20px;
  border-top: 1px solid #ccc;
}

.login-link p {
  color: #666;
  font-size: 14px;
  margin-bottom: 8px;
}

.login-link a {
  color: #2e7d32;
  text-decoration: none;
  font-weight: 600;
  font-size: 16px;
  transition: color 0.3s ease;
}

.login-link a:hover {
  color: #1b5e20;
  text-decoration: underline;
}

.error-message {
  color: #d32f2f;
  font-size: 14px;
  margin-top: 5px;
  display: flex;
  align-items: center;
  gap: 5px;
}

@media (max-width: 480px) {
  .register-container {
    padding: 20px;
    margin: 10px;
  }
  
  .register-header h1 {
    font-size: 24px;
  }

  .resend-btn, .verify-otp-btn {
    font-size: 11px;
    padding: 4px 8px;
    min-width: 50px;
  }

  .resend-btn {
    right: 60px;
  }

  .verified-badge {
    min-width: 80px;
    font-size: 11px;
  }
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>