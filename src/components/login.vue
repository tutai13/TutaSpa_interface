<template>
  <div class="spa-login-wrapper">
    <div class="spa-login-container">
      <!-- Header Section -->
      <div class="login-header">
        <div class="welcome-icon">🌿</div>
        <h1 class="login-title">Chào mừng trở lại</h1>
        <p class="login-subtitle">Đăng nhập để tiếp tục hành trình chăm sóc sức khỏe của bạn</p>
      </div>

      <!-- Login Form -->
      <form @submit.prevent="handleLogin" class="login-form">
        <!-- Phone Input -->
        <div class="form-group">
          <label for="phone" class="form-label">Số điện thoại</label>
          <div class="input-container">
            <div class="input-icon">📱</div>
            <input 
              type="tel" 
              id="phone"
              class="form-input"
              v-model="form.phone"
              placeholder="Nhập số điện thoại"
              :class="{ 'error': errors.phone }"
            >
          </div>
          <transition name="fade">
            <div v-if="errors.phone" class="error-message">
              <i class="fas fa-exclamation-circle"></i>
              {{ errors.phone }}
            </div>
          </transition>
        </div>

        <!-- Password Input -->
        <div class="form-group">
          <label for="password" class="form-label">Mật khẩu</label>
          <div class="input-container">
            <div class="input-icon">🔒</div>
            <input 
              :type="showPassword ? 'text' : 'password'"
              id="password"
              class="form-input"
              v-model="form.password"
              placeholder="Nhập mật khẩu"
              :class="{ 'error': errors.password }"
            >
            <div 
              class="password-toggle"
              @click="togglePassword"
            >
              {{ showPassword ? '🙈' : '👁️' }}
            </div>
          </div>
          <transition name="fade">
            <div v-if="errors.password" class="error-message">
              <i class="fas fa-exclamation-circle"></i>
              {{ errors.password }}
            </div>
          </transition>
        </div>

        <!-- Login Button -->
        <button 
          type="submit" 
          class="login-btn"
          :disabled="isLoading"
        >
          <div v-if="isLoading" class="btn-content">
             <div class="loading-spinner"></div>
            <span>Đang đăng nhập...</span>
          </div>
          <div v-else class="btn-content">
            <span>🌸</span>
            <span>Đăng nhập</span>
          </div>
        </button>

        <!-- Forgot Password -->
        <div class="forgot-password">
          <a href="#" @click.prevent="forgotPassword" class="forgot-link">
            Quên mật khẩu?
          </a>
        </div>

        <!-- Divider -->
        <div class="divider">
          <span>Hoặc</span>
        </div>

        <!-- Register Link -->
        <div class="register-section">
          <p class="register-text">Chưa có tài khoản?</p>
          <a href="#" @click.prevent="goToRegister" class="register-btn">
            <span>✨</span>
            <span>Đăng ký ngay</span>
          </a>
        </div>
      </form>

      <!-- Decorative Elements -->
      <div class="decorative-elements">
        <div class="leaf leaf-1">🍃</div>
        <div class="leaf leaf-2">🌱</div>
        <div class="leaf leaf-3">🌿</div>
      </div>
    </div>
  </div>
</template>

<script setup>
import notification from '../notification'
import { ref, reactive } from 'vue'
import { useRouter } from 'vue-router'
import { authAPI } from '../services/authservice' 

// Router instance
const router = useRouter()

// Reactive data
const form = reactive({
  phone: '',
  password: ''
})

const errors = reactive({
  phone: '',
  password: ''
})

// Refs
const showPassword = ref(false)
const isLoading = ref(false)

// Methods
const validateForm = () => {
  let isValid = true
  
  // Reset errors
  errors.phone = ''
  errors.password = ''

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

  return isValid
}

const handleLogin = async () => {
  if (!validateForm()) return

  isLoading.value = true

  try {
    const result = await authAPI.login({
      PhoneNumber: form.phone,
      Password: form.password
    })

    if(result){
        notification.success("Đăng nhập thành công", {
            title : "",
            timer : 1500
        })

        setTimeout(() => {
            router.push('/')
        }, 1500)
        
        return ;    
    }

    notification.error("Đăng nhập thất bại")
    
  } catch (error) {
    console.error('Login error:', error)
    notification.error( error.message ? error.message : 'Đăng nhập thất bại. Vui lòng thử lại!' )
  } finally {
    isLoading.value = false
  }
}

const togglePassword = () => {
  showPassword.value = !showPassword.value
}

const forgotPassword = () => {
  router.push('/forgot-password')
}

const goToRegister = () => {
  router.push('/register')
}

// Emit events
const emit = defineEmits(['login-success', 'forgot-password', 'go-to-register'])
</script>

<style scoped>
.spa-login-wrapper {
  min-height: 100vh;
  background: linear-gradient(135deg, #f5f9f5 0%, #e8f5e8 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem 1rem;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  position: relative;
  overflow: hidden;
}

.spa-login-wrapper::before {
  content: '';
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: radial-gradient(circle, rgba(107, 168, 107, 0.05) 0%, transparent 70%);
  animation: float 20s ease-in-out infinite;
}

@keyframes float {
  0%, 100% { transform: translate(0, 0) rotate(0deg); }
  33% { transform: translate(30px, -30px) rotate(120deg); }
  66% { transform: translate(-20px, 20px) rotate(240deg); }
}

.spa-login-container {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(20px);
  border-radius: 24px;
  box-shadow: 
    0 20px 40px rgba(45, 90, 45, 0.1),
    0 0 0 1px rgba(255, 255, 255, 0.2);
  padding: 2.5rem;
  width: 100%;
  max-width: 450px;
  position: relative;
  z-index: 2;
  overflow: hidden;
}

.spa-login-container::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, #6ba86b, #4a8c4a, #6ba86b);
  background-size: 200% 100%;
  animation: gradient 3s ease infinite;
}

@keyframes gradient {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

/* Header */
.login-header {
  text-align: center;
  margin-bottom: 2rem;
}

.welcome-icon {
  font-size: 3rem;
  margin-bottom: 1rem;
  animation: bounce 2s ease-in-out infinite;
}

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
  40% { transform: translateY(-10px); }
  60% { transform: translateY(-5px); }
}

.login-title {
  color: #2d5a2d;
  font-size: 2rem;
  font-weight: 700;
  margin-bottom: 0.5rem;
  letter-spacing: -0.02em;
}

.login-subtitle {
  color: #5a7a5a;
  font-size: 1rem;
  line-height: 1.5;
  margin-bottom: 0;
}

/* Form */
.login-form {
  position: relative;
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-label {
  display: block;
  color: #2d5a2d;
  font-weight: 600;
  margin-bottom: 0.5rem;
  font-size: 0.95rem;
}

.input-container {
  position: relative;
  display: flex;
  align-items: center;
}

.input-icon {
  position: absolute;
  left: 1rem;
  font-size: 1.2rem;
  z-index: 1;
  opacity: 0.7;
}

.form-input {
  width: 100%;
  padding: 1rem 1rem 1rem 3rem;
  border: 2px solid #e8f5e8;
  border-radius: 16px;
  font-size: 1rem;
  background: rgba(255, 255, 255, 0.8);
  transition: all 0.3s ease;
  color: #2d5a2d;
}

.form-input::placeholder {
  color: #9db29d;
}

.form-input:focus {
  outline: none;
  border-color: #6ba86b;
  background: rgba(255, 255, 255, 1);
  box-shadow: 0 0 0 4px rgba(107, 168, 107, 0.1);
  transform: translateY(-1px);
}

.form-input.error {
  border-color: #e74c3c;
  box-shadow: 0 0 0 4px rgba(231, 76, 60, 0.1);
}

.password-toggle {
  position: absolute;
  right: 1rem;
  font-size: 1.2rem;
  cursor: pointer;
  z-index: 1;
  padding: 0.25rem;
  border-radius: 50%;
  transition: all 0.3s ease;
}

.password-toggle:hover {
  background: rgba(107, 168, 107, 0.1);
  transform: scale(1.1);
}

/* Login Button */
.login-btn {
  width: 100%;
  padding: 1rem 1.5rem;
  background: linear-gradient(135deg, #6ba86b 0%, #4a8c4a 100%);
  color: white;
  border: none;
  border-radius: 16px;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-bottom: 1.5rem;
  position: relative;
  overflow: hidden;
}

.login-btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.5s ease;
}

.login-btn:hover::before {
  left: 100%;
}

.login-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 25px rgba(107, 168, 107, 0.3);
}

.login-btn:active {
  transform: translateY(0);
}

.login-btn:disabled {
  opacity: 0.7;
  cursor: not-allowed;
  transform: none;
}

.btn-content {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.loading-spinner {
  width: 20px;
  height: 20px;
  border: 2px solid rgba(255, 255, 255, 0.3);
  border-top: 2px solid white;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

/* Forgot Password */
.forgot-password {
  text-align: center;
  margin-bottom: 1.5rem;
}

.forgot-link {
  color: #6ba86b;
  text-decoration: none;
  font-size: 0.95rem;
  font-weight: 500;
  padding: 0.5rem 1rem;
  border-radius: 20px;
  transition: all 0.3s ease;
  display: inline-block;
}

.forgot-link:hover {
  background: rgba(107, 168, 107, 0.1);
  color: #4a8c4a;
  transform: scale(1.05);
}

/* Divider */
.divider {
  position: relative;
  text-align: center;
  margin: 1.5rem 0;
}

.divider::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 0;
  right: 0;
  height: 1px;
  background: linear-gradient(to right, transparent, #e8f5e8, transparent);
}

.divider span {
  background: rgba(255, 255, 255, 0.95);
  color: #5a7a5a;
  padding: 0 1rem;
  font-size: 0.9rem;
  position: relative;
}

/* Register Section */
.register-section {
  text-align: center;
}

.register-text {
  color: #5a7a5a;
  font-size: 0.95rem;
  margin-bottom: 1rem;
}

.register-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  color: #6ba86b;
  text-decoration: none;
  font-weight: 600;
  font-size: 1rem;
  padding: 0.75rem 1.5rem;
  border: 2px solid #6ba86b;
  border-radius: 16px;
  transition: all 0.3s ease;
  background: rgba(107, 168, 107, 0.05);
}

.register-btn:hover {
  background: #6ba86b;
  color: white;
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(107, 168, 107, 0.3);
}

/* Error Message */
.error-message {
  color: #e74c3c;
  font-size: 0.85rem;
  margin-top: 0.5rem;
  display: flex;
  align-items: center;
  gap: 0.3rem;
  padding: 0.5rem 0.75rem;
  background: rgba(231, 76, 60, 0.05);
  border-radius: 8px;
  border-left: 3px solid #e74c3c;
}

/* Decorative Elements */
.decorative-elements {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  pointer-events: none;
  z-index: -1;
}

.leaf {
  position: absolute;
  font-size: 1.5rem;
  opacity: 0.3;
  animation: float-leaf 6s ease-in-out infinite;
}

.leaf-1 {
  top: 20%;
  left: 10%;
  animation-delay: 0s;
}

.leaf-2 {
  top: 60%;
  right: 15%;
  animation-delay: 2s;
}

.leaf-3 {
  bottom: 30%;
  left: 20%;
  animation-delay: 4s;
}

@keyframes float-leaf {
  0%, 100% { transform: translate(0, 0) rotate(0deg); }
  25% { transform: translate(10px, -10px) rotate(90deg); }
  50% { transform: translate(-5px, 5px) rotate(180deg); }
  75% { transform: translate(5px, -5px) rotate(270deg); }
}

/* Transitions */
.fade-enter-active, .fade-leave-active {
  transition: all 0.3s ease;
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}

/* Responsive */
@media (max-width: 768px) {
  .spa-login-wrapper {
    padding: 1rem;
  }
  
  .spa-login-container {
    padding: 2rem 1.5rem;
    max-width: 100%;
  }
  
  .login-title {
    font-size: 1.7rem;
  }
  
  .form-input {
    font-size: 16px; /* Prevent zoom on iOS */
  }
}

@media (max-width: 480px) {
  .spa-login-container {
    padding: 1.5rem 1rem;
  }
  
  .login-title {
    font-size: 1.5rem;
  }
  
  .welcome-icon {
    font-size: 2.5rem;
  }
}
</style>