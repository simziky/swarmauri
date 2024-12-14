<script setup>
import { Avatar, FileUpload } from "@swarmakit/vue";
import av from "./assets/3d-avatar-male_lg.webp";
import swarm from "./assets/swa.png";
import { ref } from "vue";

const previews = ref([]);

const onFileChange = (event) => {
  const files = event.target.files;
  if (files && files.length > 0) {
    // Loop through the selected files and create object URLs
    Array.from(files).forEach((file) => {
      if (file.type.startsWith("image/")) {
        previews.value.push({
          file,
          previewUrl: URL.createObjectURL(file),
        });
      }
    });
  }
};
</script>

<template>
  <div class="main">
    <section class="avatar-section">
      <div>
        <Avatar :imageSrc="av" ariaLabel="Avatar 1" class="avimg" />
        <h3>Simeon Ehioze</h3>
      </div>

      <h1 style="font-size: 50px; font-weight: 900">X</h1>
      <div>
        <Avatar :imageSrc="swarm" ariaLabel="Avatar 2" class="av2" />
        <h3>Swarmauri</h3>
      </div>
    </section>

    <section class="image-section">
      <FileUpload
        multiple="true"
        @change="onFileChange"
        class="file"
      />
    </section>

    <section class="preview-section">
      <!-- Only render the preview section if there are images to display -->
      <div v-if="previews.length > 0" class="preview-container">
        <h4>Image Previews:</h4>
        <div class="previews">
          <div
            v-for="(preview, index) in previews"
            :key="index"
            class="preview-item"
          >
            <img
              :src="preview.previewUrl"
              :alt="'Selected preview ' + index"
              class="preview-image"
            />
            <p>{{ preview.file.name }}</p>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped lang="scss">
.main {
  width: 100%;
  height: 100%;
}

.avatar-section {
  display: flex;
  align-items: center;
  padding: 20px;
  gap: 35px;

  div {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
}

.file-upload-container {
  border: 1px dashed red;
  display: flex;
  place-items: center;
  height: 200px;
  width: 200px;

  ::v-deep(input[type="file"]) {
    display: block;
  }
}

.av2 {
  ::v-deep(.avatar) {
    background-color: black;
  }
}

.image-section {
  width: 100%;
  height: 300px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.preview-section {
  padding: 20px;
  .preview-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    .previews {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      .preview-item {
        display: flex;
        flex-direction: column;
        align-items: center;
        .preview-image {
          width: 100px;
          height: 100px;
          object-fit: cover;
          border: 1px solid #ddd;
          border-radius: 5px;
        }
        p {
          margin-top: 5px;
          font-size: 12px;
          color: #555;
        }
      }
    }
  }
}
</style>
