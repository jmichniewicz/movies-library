<template>
  <div class="infobox">
    <div class="refresh" @click="updateNetworkInfo(info)">&olarr;</div>
    <div class="close" @click.prevent="close">&times;</div>
    <p>You are connected from: <strong id="city">{{ ipinfo ? ipinfo.city : 'unknown' }}</strong>, IP: <strong>{{ ipinfo ? ipinfo.ip : 'unknown' }}</strong></p>
    <p>Your connection state is <strong id="status">unknown</strong>.</p>
    <p>Current theoretical network type is <strong id="networkType">not available</strong>.</p>
    <p>Current effective network type is <strong id="effectiveNetworkType">not available</strong>.</p>
    <p>Current max downlink speed is <strong id="downlinkMax">not available</strong> Mbps.</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Infobox',
  data() {
    return {
      info: null,
      target: null,
      ipinfo: null,
    }
  },
  computed: {
  },
  mounted() {
    this.getIpInfo();
    this.initialize();
  },
  beforeDestroy() {
    this.info.removeEventListener('change', this.updateNetworkInfo);
    window.removeEventListener('online', this.handleStateChange);
    window.removeEventListener('offline', this.handleStateChange);
  },
  methods: {
    initialize() {
      this.target = document.getElementById('status');
      this.info = this.getConnection();
      if (this.info) {
        this.info.addEventListener('change', this.updateNetworkInfo);
        this.updateNetworkInfo(this.info);
      };
      document.getElementById('status').innerHTML = navigator.onLine ? 'online' : 'offline';
      window.addEventListener('online', this.handleStateChange);
      window.addEventListener('offline', this.handleStateChange);
    },
    getConnection() {
      return navigator.connection || navigator.mozConnection ||
        navigator.webkitConnection || navigator.msConnection;
    },
    updateNetworkInfo(info) {
      this.getIpInfo();
      document.getElementById('networkType').innerHTML = info.type;
      document.getElementById('effectiveNetworkType').innerHTML = info.effectiveType;
      document.getElementById('downlinkMax').innerHTML = info.downlinkMax || info.downlink;
    },
    getIpInfo() {
      return axios.get(`https://ipinfo.io/?token=9da73fd0e057f2`)
      .then((res) => {
        this.ipinfo = res.data;
        return { ipinfo: res.data }
      })
      .catch(error => {
        console.log('There was an error:', error.response)
      })
    },
    handleStateChange() {
      // var newState = document.createElement('p');
      const state = navigator.onLine ? 'online' : 'offline';
      // newState.innerHTML = '<span class="badge">' + timeBadge + '</span> Connection state changed to <b>' + state + '</b>.';
      this.target.innerHTML = state;
      const self = this;
      if(state === 'online') {
        let timeoutID = setTimeout(function(){
          this.info = self.getConnection();
          self.updateNetworkInfo(this.info);
          window.clearTimeout(timeoutID);
        }, 1000);
      }
    },
    close() {
      this.$emit('close');
    }
  },
  transition: 'fade'
}
</script>

<style scoped>
.infobox {
  display: block;
  position: fixed;
  top: 10px;
  right: 20px;
  z-index: 2;
  /* left: calc(50% - 200px); */
  width: 325px;
  min-height: 50px;
  background: lightgreen;
  padding: 10px 30px;
  font-family: 'Blinker', sans-serif;
  font-size: 11px;
  text-align: right;
  border: 2px green solid;
  border-radius: 10px;
  transition: all 0.5s ease;
  opacity: 1;
}
.infobox.hidden {
  opacity: 0;
  right: -50px;
  z-index: 0;
}
.infobox::before {
  content: 'i';
  display: inline-block;
  position: absolute;
  top: calc(50% - 12.5px);
  left: 15px;
  border: 1px green solid;
  border-radius: 50%;
  color: green;
  width: 25px;
  height: 25px;
  font-size: 20px;
  text-align: center;
}
.close {
  position: absolute;
  top: 0px;
  right: 5px;
  font-size: 20px;
  cursor: pointer;
  transition: transform 0.3s ease-in-out;
}
.close:hover {
  transform: scale(1.25);
}
.refresh {
  position: absolute;
  bottom: 5px;
  right: 5px;
  font-size: 15px;
  max-width: 15px;
  max-height: 15px;
  cursor: pointer;
  transition: transform 0.3s ease-in-out;
}
.refresh:hover {
  transform: rotate(-360deg);
}

.fade-enter-active {
  animation: bounce-in .8s;
}
.fade-leave-active {
  animation: bounce-out .5s;
}
@keyframes bounce-in {
  0% { transform: scale(0) }
  50% { transform: scale(1.5) }
  100% { transform: scale(1) }
}
@keyframes bounce-out {
  0% { transform: scale(1) }
  50% { transform: scale(1.5) }
  100% { transform: scale(0) }
}
</style>