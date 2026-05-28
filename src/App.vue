<template>
  <div class="app">
    <!-- Header -->
    <header class="header">
      <div class="container">
        <div class="logo">
          <svg class="logo-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect x="2" y="4" width="20" height="16" rx="3" stroke="currentColor" stroke-width="2"/>
            <path d="M10 9L15 12L10 15V9Z" fill="currentColor"/>
          </svg>
          <h1>ThumbGrabber</h1>
        </div>
        <nav class="nav">
          <a href="#how-to-use">How to Use</a>
          <a href="#features">Features</a>
          <a href="#faq">FAQ</a>
        </nav>
      </div>
    </header>

    <!-- Hero Section -->
    <section class="hero">
      <div class="container">
        <h2>Download YouTube Thumbnails in HD & 4K</h2>
        <p class="hero-subtitle">Free online tool to grab high-quality thumbnails from any YouTube video. No signup required.</p>
        
        <!-- Input Section -->
        <div class="input-section">
          <div class="input-wrapper">
            <input 
              v-model="videoUrl" 
              type="text" 
              placeholder="Paste YouTube URL here (e.g., https://www.youtube.com/watch?v=...)"
              @keyup.enter="getThumbnail"
              class="url-input"
            />
            <button @click="getThumbnail" :disabled="isLoading || !videoUrl" class="get-btn">
              <span v-if="!isLoading">Get Thumbnail</span>
              <span v-else>Loading...</span>
            </button>
          </div>
          
          <div v-if="error" class="error-message">
            {{ error }}
          </div>
        </div>

        <!-- Results Section -->
        <div v-if="thumbnailData" class="results-section">
          <div class="video-info">
            <img :src="thumbnailData.maxres" alt="Video thumbnail" class="preview-image" />
            <div class="video-details">
              <h3>Thumbnail Ready!</h3>
              <p>Choose your preferred resolution:</p>
            </div>
          </div>

          <div class="resolution-grid">
            <div v-for="(url, quality) in thumbnailData" :key="quality" class="resolution-card">
              <div class="quality-badge">{{ getQualityLabel(quality) }}</div>
              <div class="resolution-size">{{ getResolutionSize(quality) }}</div>
              <a :href="url" target="_blank" rel="noopener noreferrer" class="download-btn" @click.prevent="downloadImage(url, quality)">
                <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path d="M21 15V19C21 19.5304 20.7893 20.0391 20.4142 20.4142C20.0391 20.7893 19.5304 21 19 21H5C4.46957 21 3.96086 20.7893 3.58579 20.4142C3.21071 20.0391 3 19.5304 3 19V15" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                  <path d="M7 10L12 15L17 10" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                  <path d="M12 15V3" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
                Download
              </a>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- How to Use Section -->
    <section id="how-to-use" class="how-to-section">
      <div class="container">
        <h2>How to Download YouTube Thumbnails</h2>
        <div class="steps-grid">
          <div class="step-card">
            <div class="step-number">1</div>
            <h3>Copy URL</h3>
            <p>Copy the YouTube video URL from your browser or YouTube app</p>
          </div>
          <div class="step-card">
            <div class="step-number">2</div>
            <h3>Paste & Click</h3>
            <p>Paste the URL in the input box above and click "Get Thumbnail"</p>
          </div>
          <div class="step-card">
            <div class="step-number">3</div>
            <h3>Download</h3>
            <p>Choose your preferred resolution and download the thumbnail instantly</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Features Section -->
    <section id="features" class="features-section">
      <div class="container">
        <h2>Why Choose Our Tool?</h2>
        <div class="features-grid">
          <div class="feature-card">
            <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M13 2L3 14H12L11 22L21 10H12L13 2Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
            <h3>Lightning Fast</h3>
            <p>Get thumbnails instantly with our optimized processing engine</p>
          </div>
          <div class="feature-card">
            <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M12 22C17.5228 22 22 17.5228 22 12C22 6.47715 17.5228 2 12 2C6.47715 2 2 6.47715 2 12C2 17.5228 6.47715 22 12 22Z" stroke="currentColor" stroke-width="2"/>
              <path d="M9 12L11 14L15 10" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
            <h3>100% Free</h3>
            <p>No hidden fees, no registration, completely free to use forever</p>
          </div>
          <div class="feature-card">
            <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <rect x="5" y="2" width="14" height="20" rx="2" stroke="currentColor" stroke-width="2"/>
              <path d="M12 18H12.01" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
            </svg>
            <h3>Mobile Friendly</h3>
            <p>Works perfectly on all devices - desktop, tablet, and mobile</p>
          </div>
          <div class="feature-card">
            <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M21 16V8C20.9996 7.64927 20.9071 7.30481 20.7315 7.00116C20.556 6.69751 20.3037 6.44536 20 6.27L13 2.27C12.696 2.09446 12.3511 2.00205 12 2.00205C11.6489 2.00205 11.304 2.09446 11 2.27L4 6.27C3.69626 6.44536 3.44398 6.69751 3.26846 7.00116C3.09294 7.30481 3.00036 7.64927 3 8V16C3.00036 16.3507 3.09294 16.6952 3.26846 16.9988C3.44398 17.3025 3.69626 17.5546 4 17.73L11 21.73C11.304 21.9055 11.6489 21.9979 12 21.9979C12.3511 21.9979 12.696 21.9055 13 21.73L20 17.73C20.3037 17.5546 20.556 17.3025 20.7315 16.9988C20.9071 16.6952 20.9996 16.3507 21 16Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
            <h3>Multiple Resolutions</h3>
            <p>Download in SD, HQ, HD, Full HD, and 4K quality options</p>
          </div>
        </div>
      </div>
    </section>

    <!-- FAQ Section -->
    <section id="faq" class="faq-section">
      <div class="container">
        <h2>Frequently Asked Questions</h2>
        <div class="faq-grid">
          <div class="faq-card">
            <h3>How do I download YouTube thumbnails?</h3>
            <p>Simply paste the YouTube video URL into our tool, click 'Get Thumbnail', and choose your preferred resolution to download. It's that easy!</p>
          </div>
          <div class="faq-card">
            <h3>Is this YouTube thumbnail downloader free?</h3>
            <p>Yes, our YouTube thumbnail downloader is completely free to use with no limitations or registration required. You can download as many thumbnails as you want.</p>
          </div>
          <div class="faq-card">
            <h3>What resolutions are available?</h3>
            <p>We offer multiple resolutions including SD (320x180), HQ (480x360), HD (1280x720), Full HD (1920x1080), and 4K (3840x2160) when available from the source video.</p>
          </div>
          <div class="faq-card">
            <h3>Can I use downloaded thumbnails for my videos?</h3>
            <p>You can use downloaded thumbnails for personal projects, but make sure to respect copyright laws and YouTube's terms of service when using them commercially.</p>
          </div>
          <div class="faq-card">
            <h3>Does this work on mobile devices?</h3>
            <p>Absolutely! Our tool is fully responsive and works perfectly on smartphones, tablets, and desktop computers across all major browsers.</p>
          </div>
          <div class="faq-card">
            <h3>Why isn't the 4K thumbnail showing?</h3>
            <p>Not all YouTube videos have 4K thumbnails available. The availability depends on the original video upload and what YouTube has generated for that specific video.</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="footer">
      <div class="container">
        <div class="footer-content">
          <div class="footer-brand">
            <div class="logo">
              <svg class="logo-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <rect x="2" y="4" width="20" height="16" rx="3" stroke="currentColor" stroke-width="2"/>
                <path d="M10 9L15 12L10 15V9Z" fill="currentColor"/>
              </svg>
              <span>ThumbGrabber</span>
            </div>
            <p>The fastest and easiest way to download YouTube thumbnails in high quality.</p>
          </div>
          <div class="footer-links">
            <div class="link-group">
              <h4>Quick Links</h4>
              <a href="#how-to-use">How to Use</a>
              <a href="#features">Features</a>
              <a href="#faq">FAQ</a>
            </div>
            <div class="link-group">
              <h4>Legal</h4>
              <a href="#">Privacy Policy</a>
              <a href="#">Terms of Service</a>
              <a href="#">Contact</a>
            </div>
          </div>
        </div>
        <div class="footer-bottom">
          <p>&copy; 2025 ThumbGrabber. All rights reserved. Not affiliated with YouTube.</p>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const videoUrl = ref('')
const thumbnailData = ref(null)
const isLoading = ref(false)
const error = ref('')

const extractVideoId = (url) => {
  const patterns = [
    /(?:youtube\.com\/watch\?v=|youtu\.be\/)([^&\n?#]+)/,
    /youtube\.com\/embed\/([^&\n?#]+)/,
    /youtube\.com\/v\/([^&\n?#]+)/,
  ]
  
  for (const pattern of patterns) {
    const match = url.match(pattern)
    if (match && match[1]) {
      return match[1]
    }
  }
  return null
}

const getThumbnail = () => {
  error.value = ''
  thumbnailData.value = null
  
  if (!videoUrl.value.trim()) {
    error.value = 'Please enter a YouTube URL'
    return
  }
  
  const videoId = extractVideoId(videoUrl.value)
  
  if (!videoId) {
    error.value = 'Invalid YouTube URL. Please check and try again.'
    return
  }
  
  isLoading.value = true
  
  // Simulate a brief loading time for better UX
  setTimeout(() => {
    thumbnailData.value = {
      maxres: `https://img.youtube.com/vi/${videoId}/maxresdefault.jpg`,
      sddefault: `https://img.youtube.com/vi/${videoId}/sddefault.jpg`,
      hqdefault: `https://img.youtube.com/vi/${videoId}/hqdefault.jpg`,
      mqdefault: `https://img.youtube.com/vi/${videoId}/mqdefault.jpg`,
      default: `https://img.youtube.com/vi/${videoId}/default.jpg`,
    }
    isLoading.value = false
  }, 500)
}

const getQualityLabel = (quality) => {
  const labels = {
    maxres: '4K / Max Resolution',
    sddefault: 'SD Quality',
    hqdefault: 'HQ Quality',
    mqdefault: 'MQ Quality',
    default: 'Default'
  }
  return labels[quality] || quality
}

const getResolutionSize = (quality) => {
  const sizes = {
    maxres: '3840 x 2160',
    sddefault: '640 x 480',
    hqdefault: '480 x 360',
    mqdefault: '320 x 180',
    default: '120 x 90'
  }
  return sizes[quality] || 'Unknown'
}

const downloadImage = async (url, quality) => {
  try {
    const response = await fetch(url)
    const blob = await response.blob()
    const downloadUrl = window.URL.createObjectURL(blob)
    const a = document.createElement('a')
    a.href = downloadUrl
    a.download = `thumbnail-${quality}.jpg`
    document.body.appendChild(a)
    a.click()
    document.body.removeChild(a)
    window.URL.revokeObjectURL(downloadUrl)
  } catch (err) {
    // Fallback: open in new tab
    window.open(url, '_blank')
  }
}
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

/* Header */
.header {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  position: sticky;
  top: 0;
  z-index: 100;
  box-shadow: 0 2px 20px rgba(0, 0, 0, 0.1);
}

.header .container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 20px;
}

.logo {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #667eea;
}

.logo-icon {
  width: 32px;
  height: 32px;
}

.logo h1 {
  font-size: 24px;
  font-weight: 700;
}

.nav {
  display: flex;
  gap: 30px;
}

.nav a {
  color: #333;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s;
}

.nav a:hover {
  color: #667eea;
}

/* Hero Section */
.hero {
  padding: 80px 0;
  color: white;
}

.hero h2 {
  font-size: 48px;
  text-align: center;
  margin-bottom: 15px;
  font-weight: 800;
}

.hero-subtitle {
  text-align: center;
  font-size: 18px;
  opacity: 0.95;
  margin-bottom: 50px;
}

.input-section {
  max-width: 800px;
  margin: 0 auto;
}

.input-wrapper {
  display: flex;
  gap: 15px;
  background: white;
  padding: 10px;
  border-radius: 15px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
}

.url-input {
  flex: 1;
  border: none;
  padding: 15px 20px;
  font-size: 16px;
  outline: none;
  border-radius: 10px;
}

.get-btn {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  padding: 15px 40px;
  font-size: 16px;
  font-weight: 600;
  border-radius: 10px;
  cursor: pointer;
  transition: transform 0.3s, box-shadow 0.3s;
}

.get-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 5px 20px rgba(102, 126, 234, 0.4);
}

.get-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.error-message {
  background: rgba(255, 255, 255, 0.2);
  color: white;
  padding: 15px;
  border-radius: 10px;
  margin-top: 15px;
  text-align: center;
}

/* Results Section */
.results-section {
  max-width: 1000px;
  margin: 50px auto 0;
  background: white;
  border-radius: 20px;
  padding: 40px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
}

.video-info {
  display: flex;
  gap: 30px;
  align-items: center;
  margin-bottom: 40px;
}

.preview-image {
  width: 320px;
  border-radius: 15px;
  box-shadow: 0 5px 20px rgba(0, 0, 0, 0.2);
}

.video-details h3 {
  color: #667eea;
  font-size: 28px;
  margin-bottom: 10px;
}

.video-details p {
  color: #666;
}

.resolution-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 20px;
}

.resolution-card {
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  padding: 25px;
  border-radius: 15px;
  text-align: center;
  transition: transform 0.3s;
}

.resolution-card:hover {
  transform: translateY(-5px);
}

.quality-badge {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 8px 15px;
  border-radius: 20px;
  font-weight: 600;
  margin-bottom: 10px;
  display: inline-block;
}

.resolution-size {
  color: #666;
  margin-bottom: 15px;
  font-size: 14px;
}

.download-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: white;
  color: #667eea;
  padding: 12px 25px;
  border-radius: 10px;
  text-decoration: none;
  font-weight: 600;
  transition: all 0.3s;
  cursor: pointer;
  border: 2px solid #667eea;
}

.download-btn:hover {
  background: #667eea;
  color: white;
}

.download-btn svg {
  width: 20px;
  height: 20px;
}

/* How to Use Section */
.how-to-section {
  background: white;
  padding: 80px 0;
}

.how-to-section h2 {
  text-align: center;
  font-size: 36px;
  color: #333;
  margin-bottom: 50px;
}

.steps-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 30px;
}

.step-card {
  text-align: center;
  padding: 30px;
  background: #f8f9ff;
  border-radius: 15px;
}

.step-number {
  width: 60px;
  height: 60px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  font-weight: 700;
  margin: 0 auto 20px;
}

.step-card h3 {
  color: #333;
  margin-bottom: 10px;
}

.step-card p {
  color: #666;
  line-height: 1.6;
}

/* Features Section */
.features-section {
  padding: 80px 0;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.features-section h2 {
  text-align: center;
  color: white;
  font-size: 36px;
  margin-bottom: 50px;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 30px;
}

.feature-card {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  padding: 30px;
  border-radius: 15px;
  text-align: center;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.feature-card svg {
  width: 48px;
  height: 48px;
  color: white;
  margin-bottom: 20px;
}

.feature-card h3 {
  color: white;
  margin-bottom: 10px;
}

.feature-card p {
  color: rgba(255, 255, 255, 0.9);
  line-height: 1.6;
}

/* FAQ Section */
.faq-section {
  background: white;
  padding: 80px 0;
}

.faq-section h2 {
  text-align: center;
  font-size: 36px;
  color: #333;
  margin-bottom: 50px;
}

.faq-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 25px;
}

.faq-card {
  background: #f8f9ff;
  padding: 25px;
  border-radius: 15px;
  border-left: 4px solid #667eea;
}

.faq-card h3 {
  color: #667eea;
  margin-bottom: 10px;
}

.faq-card p {
  color: #666;
  line-height: 1.6;
}

/* Footer */
.footer {
  background: #1a1a2e;
  color: white;
  padding: 60px 0 30px;
}

.footer-content {
  display: grid;
  grid-template-columns: 2fr 1fr 1fr;
  gap: 50px;
  margin-bottom: 40px;
}

.footer-brand .logo {
  color: white;
  margin-bottom: 15px;
}

.footer-brand p {
  color: rgba(255, 255, 255, 0.7);
  line-height: 1.6;
}

.link-group h4 {
  margin-bottom: 15px;
  font-size: 16px;
}

.link-group a {
  display: block;
  color: rgba(255, 255, 255, 0.7);
  text-decoration: none;
  margin-bottom: 10px;
  transition: color 0.3s;
}

.link-group a:hover {
  color: #667eea;
}

.footer-bottom {
  text-align: center;
  padding-top: 30px;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  color: rgba(255, 255, 255, 0.5);
}

/* Responsive Design */
@media (max-width: 768px) {
  .hero h2 {
    font-size: 32px;
  }
  
  .input-wrapper {
    flex-direction: column;
  }
  
  .get-btn {
    width: 100%;
  }
  
  .video-info {
    flex-direction: column;
  }
  
  .preview-image {
    width: 100%;
    max-width: 320px;
  }
  
  .nav {
    display: none;
  }
  
  .footer-content {
    grid-template-columns: 1fr;
    gap: 30px;
  }
}

@media (max-width: 480px) {
  .hero {
    padding: 40px 0;
  }
  
  .hero h2 {
    font-size: 24px;
  }
  
  .resolution-grid {
    grid-template-columns: 1fr;
  }
}
</style>
