<script lang="ts">
import moment from "moment";
import { useStore } from "vuex";
import { computed, ref } from "vue";
import router from "@/router";

export default {
  props: {
    post: {
      type: Object,
      required: true,
      default: () => ({})
    }
  },
  setup(props: any) {
    const store = useStore();
    const user = computed(() => store.state.loggedUser);
    const showOptions = ref(false);
    if (props.post.author.id === user.value._id) showOptions.value = true;

    function deletePost() {
      console.log(props.post._id);
      store.dispatch("posts/deleteAPost", props.post._id);
    }

    function goToEdit() {
      if (showOptions.value) {
        router.push({ name: "EditPost", params: { id: props.post._id } });
      }
    }

    function goToDetails(): void {
      router.push({ name: "Details", params: { id: props.post._id } });
    }

    const timeStart = moment(moment(props.post.timeStart).format()).format(
      "HH:mm"
    );
    const timeEnd = moment(moment(props.post.timeEnd).format()).format("HH:mm");

    return {
      timeStart,
      timeEnd,
      showOptions,
      deletePost,
      goToEdit,
      goToDetails
    };
  }
};
</script>

<template>
  <div class="post">
    <div class="row">
      <div class="col-lg-11 main" @click="goToDetails">
        <div class="ride-info">
          <h5 class="grow">{{ post.cityStart }} - {{ post.cityEnd }}</h5>
          <h6 class="grow">{{ timeStart }} - {{ timeEnd }}</h6>
        </div>
        <div class="author-info">
          <h6>{{ post.author.firstName }} {{ post.author.lastName }}</h6>
          <h6>{{ post.author.rating }} stars</h6>
        </div>
      </div>
      <div class="col-lg-1">
        <div class="options ">
          <button
            v-if="showOptions"
            class="btn btn-sm btn-info"
            @click="goToEdit"
          >
            <span class="material-icons">edit</span>
          </button>
          <button
            v-if="showOptions"
            class="btn btn-sm btn-danger"
            @click="deletePost"
          >
            <span class="material-icons">delete</span>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import "../assets/styles/variables";

.post {
  height: 100px;
  background: #fff;
  margin-bottom: 20px;
  border: 1px solid rgb(206, 206, 206);
  box-shadow: 3px 5px 5px rgb(206, 206, 206);
  border-radius: 12px;
  padding: 16px 20px;

  .row {
    padding: 16px, 20px;
    height: 100%;
    z-index: 1;
  }

  span {
    display: block;
  }

  &:hover {
    background-color: $secondary-color;
    cursor: pointer;
  }
}

.main {
  display: flex;
  align-items: center;
  text-align: start;
  padding-right: 0px;
}

.ride-info {
  display: inline-block;
  width: 50%;
  justify-content: space-around;
  flex-direction: column;
  align-self: center;
  text-align: start;

  h5,
  h6 {
    margin: 0 0 4px 32px;
  }
}

.author-info {
  width: 50%;

  display: inline-block;
  flex-direction: column;
  align-self: center;
  text-align: end;
  padding-right: 0px;

  h5,
  h6 {
    margin: 0 12px 0 0;
  }
}

.options {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-right: 12px;
  z-index: 10;

  span {
    font-size: 16px;
    right: 12px;
    text-align: center;
  }

  button:nth-child(1) {
    margin-top: 4px;
    margin-bottom: 4px;
  }
}

.col-1 {
  align-content: center;
}
</style>
