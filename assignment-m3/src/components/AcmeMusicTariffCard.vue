<template>
  <div class="assignment-2">
    <h1>Assignment 2</h1>
    <div class="acme-card">
      <div class="background">
        <div class="content">
          <div class="ticket-cutout"></div>
          <div class="icon-play-container">
            <img class="icon-play" src=@/assets/play-button.svg />
          </div>
          <div class="card-title">Free Access to Acme Music</div>
          <div class="card-details">
            <div class="details-element">
              <div class="icon-container">
                <img class="icon-check" src=@/assets/check.svg />
              </div>
              <div class="details-container">
                <span class="details-info">Stream 60 million songs on your device</span>
              </div>
            </div>
            <div class="details-element">
              <div class="icon-container">
                <img class="icon-cross" src=@/assets/cross.svg />
              </div>
              <div class="details-container">
                <span class="details-info">No internet access</span>
              </div>
            </div>
          </div>
        </div>
        <div class="connect-now">
          <span>Connect now</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AcmeMusicTariffCard",
  props: {
    msg: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.assignment-2 {
  background-color: #dddd;
}

.icon-play {
  margin: 24px 0;
  width: 50px;
  height: 50px;
}

.card-title, .connect-now {
  color: #222556;
  font-weight: bold;
  font-size: 24px;
  letter-spacing: -1.5px;
  max-width: 150px;
  margin: auto;
}

.card-title:after {
  content: "®";
  font-size: 12px;
  vertical-align: top;
}

.card-details {
  margin-top: 16px;
}

.details-element {
  text-align: left;
  padding: 0 36px;
  display: flex;
  flex-direction: row;
  align-items: center;
  margin-bottom: 8px;
}

.icon-check,
.icon-cross {
  color: #fde8a6;
  width: 30px;
  height: 30px;
  margin-right: 8px;
}

.details-info {
  font-size: 18px;
}

.connect-now {
  display: flex;
  justify-content: center;
  align-items: center;
}

.background {
  padding-top: 16px;
  max-width: 350px;
  height: 400px;
  margin: auto;
  border-radius: 10px;
  background: linear-gradient(
    45deg,
    rgba(182, 240, 199, 1) 0%,
    rgba(97, 206, 195, 1) 100%
  );
  display: flex;
  flex-direction: column;
  box-shadow: 0 0 15px -5px #333;
}

.content {
  background: white;
  position: relative;
  flex-grow: 1;
  overflow: hidden;
}

.ticket-cutout::before,
.ticket-cutout::after {
  position: absolute;
  width: 24px;
  height: 24px;
  background-color: #ccc;
  border-radius: 50%;
  content: "";
  z-index: 2;
  top: 30%;
  box-shadow: inset 0 0 15px -5px #333;
}

.ticket-cutout::before {
  left: -16px;
}

.ticket-cutout::after {
  right: -16px;
}

.connect-now {
  height: 80px;
}

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
