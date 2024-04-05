<template>
  <div>
    <h1>Beer label maker</h1>
    <p>Create your custom texts on you beer label, or any other image</p>
  </div>
  <div>
    <FileUpload />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import FileUpload from './components/FileUpload.vue';

export default defineComponent({
    name: 'App',
    components: {
        FileUpload
    }
});
</script>

<style lang="scss">

</style>
