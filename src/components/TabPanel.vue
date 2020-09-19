<template>
  <a-tabs type="card" default-active-key="1">
    <a-tab-pane key="1">
      <span slot="tab">
        <a-icon type="picture" />Explore in Pinterest
      </span>

      <h1>Search...</h1>
      <a-input-search
        placeholder="input search text"
        enter-button="Search"
        size="large"
        @search="onSearch"
      />
      <a-spin :spinning="spinning" :delay="delayTime">
        <stack :column-min-width="300" :gutter-width="15" :gutter-height="15" monitor-images-loaded>
          <stack-item v-for="(image, i) in images" :key="i" style="transition: transform 300ms">
            <img :src="image.urls.small" :alt="image.alt_description" />
          </stack-item>
        </stack>
        <a-button type="link" @click="getData">Show more...</a-button>
      </a-spin>
    </a-tab-pane>
    <a-tab-pane key="2">
      <span slot="tab">
        <a-icon type="ordered-list" />Todo List
      </span>
      <h1>TODO List</h1>
      <div class="row">
        <div class="column">
          <a-input v-model="inputTitle" placeholder="Input Title" />
          <a-input v-model="inputDescription" placeholder="Input description" />
        </div>
        <a-button @click="addTODO">ADD</a-button>
      </div>
      <div class="card-wrapper">
        <a-card
          style="width: 300px; margin: 1rem 0;"
          v-for="(item, index) in todoList"
          :key="index"
          :title="`TODO ${(index+1)} - ${item.title}`"
        >
          <a slot="extra" @click="deleteTODO(item)">Delete</a>
          <div>{{ item.description }}</div>
        </a-card>
      </div>
    </a-tab-pane>
    <a-tab-pane key="3">
      <span slot="tab">
        <a-icon type="calendar" />Calendar
      </span>
      <h1>Calendar</h1>
      <a-calendar @panelChange="onPanelChange" />
    </a-tab-pane>
  </a-tabs>
</template>
      
<script>
import axios from "axios";
import { Stack, StackItem } from "vue-stack-grid";

export default {
  components: {
    Stack,
    StackItem
  },
  data: () => ({
    images: [],
    searchValue: '',
    spinning: false,
    delayTime: 100,
    perPage: 0,
    inputTitle: "",
    inputDescription: "",
    todoList: [],
  }),
  methods: {
    getData() {
      this.perPage += 20;
      axios
        .get(
          `https://api.unsplash.com/search/photos?query=${this.searchValue}&per_page=${this.perPage}`,
          {
            headers: {
              Authorization:
                "Client-ID CIBJRbDzl8Qa3MpFPujnlmeuPi6_stfnUsUGMTTGBb0",
              "Accept-Version": "v1"
            }
          }
        )
        .then(response => {
          this.images = response.data.results;
          this.spinning = false;
        })
        .catch(() => {
          this.images = [];
          this.spinning = false;
        });
    },
    onSearch(value) {
      this.images = [];
      this.spinning = true;
      this.perPage = 0;
      this.searchValue = value;
      this.getData();
    },
    addTODO: function () {
      this.todoList = this.todoList.concat(({ title: this.inputTitle, description: this.inputDescription }))
      this.inputTitle = ""
      this.inputDescription = ""
    },
    deleteTODO: function (item) {
      this.todoList = this.todoList.filter(x => x !== item)
    },
    onPanelChange(value, mode) {
      console.log(value, mode);
    },
  }
}
</script>

<style scoped>
img {
  border-radius: 20px;
  width: 100%;
  height: auto;
}
</style>