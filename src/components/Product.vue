<template>
  <div id="gridContainer" @keyup.27="hideVideo">
    <input
      id="userInput"
      placeholder="Paste any YouTube URL here (Ctrl + V)"
      type="text"
      v-if="!isLoaded"
      v-model="userInput"
      @keypress.enter="handleInput"
      @drop="handleInput"
      @paste="handleInput"
      autofocus
      tabindex="1"
    />

    <transition name="video" key="forVid">
      <iframe v-if="isLoaded" allowfullscreen="allowFullScreen" class="video" :src="src"></iframe>
    </transition>

    <transition name="button" key="forBtn">
      <button
        v-if="isLoaded"
        id="closeButton"
        @click="hideVideo"
        tabindex="2"
        @keyup.27="hideVideo"
      >
        <img src="../close.svg" width="15px" />
      </button>
    </transition>

    <p v-show="invalidUrl" class="error">Please enter a valid URL from YouTube</p>
  </div>
</template>

<script>
export default {
  name: "Product",
  data() {
    return {
      userInput: "",
      src: "",
      isLoaded: false,
      invalidUrl: false
    };
  },

  methods: {
    handleInput(e) {
      let videoId = "";
      let url = this.userInput;
      this.userInput = "";

      let isLongUrl = url.startsWith("https://www.youtube.com/");
      let isShortUrl = url.startsWith("https://youtu.be/");
      let isWithoutW = url.startsWith("https://youtube.com/");

      if (!isLongUrl && !isShortUrl && !isWithoutW) {
        if (e.key == "Enter") {
          this.invalidUrl = true;
        }
      } else {
        if (isLongUrl) {
          let vidIdWithParams = url.split(
            "https://www.youtube.com/watch?v="
          )[1];
          videoId = vidIdWithParams.split("&")[0];
        } else if (isShortUrl) {
          videoId = url.split("https://youtu.be/")[1];
        } else {
          let vidIdWithParams = url.split("https://youtube.com/watch?v=")[1];
          videoId = vidIdWithParams.split("&")[0];
        }

        this.src = `https://www.youtube-nocookie.com/embed/${videoId}?ecver=1&amp;cc_load_policy=1&amp;iv_load_policy=3&amp;rel=0&amp;yt:stretch=16:9&amp;autohide=1&amp;autoplay=1&amp;color=white`;

        this.invalidUrl = false;
        this.isLoaded = true;
      }
    },
    hideVideo() {
      this.isLoaded = false;
      this.userInput = "";
    }
  }
};
</script>

<style>
body {
  background-color: rgb(36, 36, 36);
  margin: 0;
  padding: 0;
}

#gridContainer {
  display: grid;
  grid-template-columns: 80px 1fr 1fr 1fr 80px;
  grid-template-rows: 60px 1fr 1.5fr 1fr 90px 50px;
  height: 100vh;
}

#userInput {
  border: none;
  grid-column: 2/5;
  grid-row: 2/6;
  color: rgb(138, 138, 138);
  background-color: rgb(36, 36, 36);
  font-size: 1.5em;
  font-family: Inter;
  caret-color: transparent;
  text-align: center;
}

.video {
  grid-column: 3/4;
  grid-row: 3/4;
  text-align: center;
  border: none;
  box-shadow: 0 0 25px rgba(0, 0, 0, 0.418);
  background-color: rgb(117, 117, 117);
  border-radius: 5px;
  transform: scale(2);
}

#closeButton {
  border-radius: 50%;
  padding: 10px;
  background-color: rgb(61, 61, 61);
  border: 1px solid rgb(61, 61, 61);
  box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.109);
  font-family: Dyuthi;
  width: 50px;
  height: 50px;
  margin: auto;
  text-align: center;
  font-size: 30px;
  cursor: pointer;
  transition: 200ms ease;
  grid-column: 3/4;
  grid-row: 5/6;
}

.video-enter-active {
  animation: intro 400ms ease-out;
}

.video-leave-active {
  animation: intro 400ms cubic-bezier(0.6, 0.04, 0.98, 0.335) reverse;
}

.button-enter-active {
  animation: buttonPop 400ms cubic-bezier(0.075, 0.82, 0.165, 1);
}

.button-leave-active {
  animation: buttonPop 400ms cubic-bezier(0.6, 0.04, 0.98, 0.335) reverse;
}

@keyframes intro {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@keyframes buttonPop {
  0% {
    opacity: 0;
    transform: translateY(30px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

#closeButton:hover {
  background-color: rgb(66, 66, 66);
}

.error {
  text-align: center;
  grid-column: 2/5;
  font-family: Inter;
  color: rgb(255, 97, 97);
  margin-top: 60px;
}

@media (max-width: 1080px) {
  #gridContainer {
    grid-template-columns: 10px 1fr 1.5fr 1fr 10px;
  }
  .video {
    width: 100%;
  }
  #userInput {
    font-size: 1em;
  }
}
</style>