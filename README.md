<img width="732" height="520" alt="image" src="https://github.com/user-attachments/assets/c46326a0-fde0-450f-8fc8-4b0c08875490" />
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<style>
.social {
  display: flex;
  gap: 14px;
  align-items: center;
}

/* Ortak ikon kutusu */
.icon-box {
  width: 44px;
  height: 44px;
  border-radius: 12px;
  background: #1a1a1a;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: 0.25s ease;
  cursor: pointer;
}

.icon-box:hover {
  background: linear-gradient(135deg, #7c4bff, #5f6cff);
  transform: translateY(-3px);
  box-shadow: 0px 6px 20px rgba(132, 99, 255, 0.45);
}

.icon-box img {
  width: 22px;
  height: 22px;
  filter: invert(1); /* ikonları beyaz yapar */
}
</style>
</head>

<body style="background:#0e0e0e; padding:40px;">

<div class="social">

  <!-- Telegram -->
  <a href="https://t.me/KULLANICI_ADIN" target="_blank">
    <div class="icon-box">
      <img src="https://cdn.jsdelivr.net/gh/simple-icons/simple-icons/icons/telegram.svg">
    </div>
  </a>

  <!-- Instagram -->
  <a href="https://instagram.com/KULLANICI_ADI" target="_blank">
    <div class="icon-box">
      <img src="https://cdn.jsdelivr.net/gh/simple-icons/simple-icons/icons/instagram.svg">
    </div>
  </a>

</div>

</body>
</html>
