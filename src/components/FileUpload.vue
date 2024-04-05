<template>
  <div>
    <h2>Upload your beer label (or other picture)</h2>
    <input
      type="file"
      name="file"
      @change="changeFile"
    >
    <a
      v-if="fileToShow"
      ref="downloadButton"
      href="#"
      @click="touched = false"
    >
      Download image
    </a>
    <div
      v-if="fileToShow"
      ref="image"
      class="image-container"
    >
      <img
        :src="fileToShow"
      >
      <input
        v-model="bannerText"
        :class="{ 'hidden': touched }"
        type="text"
        placeholder="Enter your text here"
        @input="onInput"
      >
      <h3>{{ bannerText }}</h3>
    </div>
  </div>
</template>

<script>
import { unref, ref, nextTick } from 'vue';
import html2canvas from 'html2canvas';

export default {
    name: 'FileUpload',
    setup() {
        const fileToShow = ref(null);
        const downloadButton = ref(null)
        const image = ref(null)
        const bannerText = ref('')
        const touched = ref(false)

        const changeFile = async (event) => {
            const files = event.target.files;
            if (files.length) {
                fileToShow.value = await readFile(files[0]);
                await nextTick();
                saveImage();
            }
        };

        const onInput = () => {
            touched.value = true;
            saveImage();
        };

        const readFile = async (file) => {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = (event) => resolve(event.target.result);
                reader.onerror = (error) => reject(error);
                reader.readAsDataURL(file);
            });
        };

        const saveImage = async () => {
            const imageDiv = unref(image);
            const canvas = await html2canvas(imageDiv);
            const imageData = canvas.toDataURL('image/png');
            const donwloadLink = unref(downloadButton);

            donwloadLink.href = imageData;
            donwloadLink.download = 'beer-label.png';
        };

        return {
            bannerText,
            downloadButton,
            fileToShow,
            image,
            touched,
            changeFile,
            onInput,
            saveImage
        };
    }
};
</script>

<style scoped>
/* Your component's style code here */
img {
  width: 100%;
}

.image-container {
  position: relative;
}

input[type="text"], h3 {
  background-color: transparent;
  font-size: 60px;
  overflow: visible;
  position: absolute;
  top: 30%;
  left: 40%;
}
.hidden {
  opacity: 0;
}
</style>