<template>
  <div class="modal" @click.self="closeModal" v-if="open">
    <div class="modal__container">
      <div class="modal__content">
        <div class="modal__up_line"></div>
        <div class="modal__close" @click="closeModal">
          <span class="modal__close_btn">x</span>
        </div>
        <div class="modal__rating">
          <h2 class="modal__rating-title">Добавить отзыв</h2>
          <p class="modal__rating-subtitle">Оценка</p>
          <div class="modal__rating__star">
            <RatingStar />
          </div>
          <textarea class="modal__rating-text" />
          <div class="modal_upload_images">
            <label class="modal__file-upload">
              <input
                @change="uploadImage"
                ref="userImages"
                type="file"
                multiple
              />
              Выберите файл
            </label>
            <button
              v-if="preview.length"
              class="modal__file-clear"
              @click="clearImages"
            >
              x
            </button>
          </div>
          <div class="modal__images" v-if="preview">
            <img
              v-for="url in preview"
              :key="url"
              :src="url"
              class="modal__images_preview"
            />
          </div>
        </div>
        <div class="modal__btn">
          <button class="modal__send_feedback">Оставить отзыв</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import RatingStar from "@/components/UI/Rating/Rating.vue";

export default {
  name: "FeedbackModal",
  components: {
    RatingStar,
  },
  data() {
    return {
      open: false,
      rating: 0,
      preview: [],
    };
  },
  methods: {
    closeModal() {
      this.open = false;
      this.clearImages();
      this.rating = 0;
    },
    uploadImage(e) {
      const file = e.target.files;
      for (let el of file) {
        let prev = URL.createObjectURL(el);
        this.preview.push(prev);
      }
    },
    clearImages() {
      this.$refs.userImages.value = "";
      this.preview = [];
    },
  },
  mounted() {
    this.$root.$on("openModal", () => {
      this.open = true;
    });
    this.$root.$on("updateStars", (star) => {
      this.rating = star;
    });
  },
  beforeDestroy() {
    this.closeModal();
  },
};
</script>

<style lang="scss">
.modal {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 10;
  background: rgba(44, 62, 80, 0.5);
  overflow: auto;
  &__content {
    position: absolute;
    left: 0;
    right: 0;
    top: 70px;
    margin-left: auto;
    margin-right: auto;
    z-index: 20;
    min-width: 360px;
    max-width: 650px;
    height: 503px;
    background-color: #ffffff;
    border-radius: 20px;
    overflow-y: none;
    ::-webkit-scrollbar {
      width: 1px;
    }
    ::-webkit-scrollbar-thumb {
      background: rgba(175, 175, 175, 0);
      border-radius: 10px 28px 28px 10px;
    }
  }
  &__close {
    display: flex;
    align-items: center;
    justify-content: center;
    align-items: center;
    position: absolute;
    top: 25px;
    right: 20px;
    width: 25px;
    height: 25px;
    background-color: #f5f5f5;
    padding: 4px;
    border-radius: 50px;
    color: #000000;
    cursor: pointer;
    &_btn {
      font-weight: 700;
    }
  }
  &__up_line {
    display: none;
    position: absolute;
    top: 25px;
    left: 148px;
    width: 89px;
    border-bottom: 3px solid #000000;
    @media (max-width: 375px) {
      display: block;
    }
  }
  &__rating {
    margin: 50px 20px;
  }
  &__rating-title {
    margin-bottom: 20px;
  }
  &__rating-subtitle {
    margin-bottom: 15px;
  }
  &__rating-text {
    border-radius: 20px;
    border: 2px solid #1fafaa;
    height: 100px;
    outline: none;
    resize: none;
    padding: 8px;
    width: 100%;
    color: #787878;
    margin-bottom: 15px;
  }
}

.modal__rating__star {
  margin-bottom: 15px;
}

.modal__file-upload {
  input[type="file"] {
    display: none;
  }
  background-color: #fed42b;
  padding: 8px 26px;
  border-radius: 10px;
}
.modal__images {
  display: flex;
  margin-top: 18px;
  &_preview {
    object-fit: cover;
    width: 50px;
    height: 50px;
    margin-right: 8px;
  }
}

.modal__btn {
  display: flex;
  justify-content: center;
}

.modal__send_feedback {
  position: absolute;
  bottom: 25px;
  display: flex;
  width: 100%;
  color: #ffffff;
  background-color: #1fafaa;
  padding: 8px 14px;
  border-radius: 10px;
  justify-content: center;
  width: 250px;
}

.modal__file-clear {
  background-color: #ff6a6a;
  padding: 2px;
  border-radius: 15px;
  width: 28px;
  height: 28px;
  color: #ffffff;
  margin-left: 18px;
}
</style>
