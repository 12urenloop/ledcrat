<template>
<keep-alive>
  <transition name="fade" mode="out-in">
    <component :is="component" :data="data" v-if="component" />
  </transition>
</keep-alive>
</template>
<script>
import Vue from 'vue';
import fs from 'fs';

export default {
  name: 'dynamic-component',
  props: ['data', 'type'],
  data() {
    return {
      component: null,
    }
  },
  methods: {
    loadData: function () {
      this.axios.get("http://localhost:3000")
      .then((response) => {
        if(this.last != response.data){
          this.last = response.data;
          this.component = Vue.component("blah", {
            template: response.data
          });
        }
      }).catch(e => {
        this.template ='<img src="http://localhost:8080/12urenloop.png" style="height: 100%; width: auto"/>';
          if(this.last != this.template){
          this.last = this.template;
            this.component = Vue.component("blah", {
              template: this.template
            });
          }
      });
    }
  },
  mounted() {
    this.loadData();
    setInterval(function () {
      this.loadData();
    }.bind(this), 2000);
    /*
      this.loader()
          .then(() => {
              this.component = () => this.loader()
          })
          .catch(() => {
              this.component = () => import('./templates/default.vue')
          })*/
  }
}
</script>
<style>
img {
  width: 100%;
  margin: auto;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 1s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
