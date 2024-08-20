<template>
  <div class="player-info d-flex align-items-center">
    <div class="avatar">
      <img src="@/assets/images/avatar.png" alt="Avatar" />
    </div>
    <div class="details">
      <h4>{{ player.nickname }}</h4>
      <p>{{ player.position }}</p>
    </div>
    <div class="ml-auto">
      <p>...</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PlayerInfo',

  props: {
    player: {
      type: Object,
      required: true
    }
  }
}
</script>

<style scoped>
.player-info {
  background-color: #003957;
  padding: 10px;
  border-radius: 10px;
  cursor: pointer;
}
.player-info:hover {
  background-color: #014f79;
}
.avatar img {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  margin-right: 10px;
}
.details h4 {
  margin: 0;
  text-transform: uppercase;
  font-weight: bold;
  color: #cfd9dc;
  font-size: clamp(14px, 1.5vw, 18px);
}

.details p {
  margin: 0;
  font-style: italic;
  color: #899f9e;
  font-size: clamp(12px, 1.2vw, 16px);
}
.ml-auto {
  margin-left: auto;
  cursor: pointer;
  color: #fff;
}
</style>
