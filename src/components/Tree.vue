<template>
  <ul>
    <li v-for="service in itemsList" :key="service.id" @click.stop="toggle(service)">

      <span class="service-name" :class="{ 'pointer': service.children?.length }">
        <svg v-if="service.children?.length" width="24" height="24" viewBox="0 0 22 22" fill="none" class="rotate-base"
          :class="{ 'rotate': service.isToggle }" xmlns="http://www.w3.org/2000/svg">
          <path d="M4.58337 13.75L11 7.33337L17.4167 13.75" stroke="#111827" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" />
        </svg>
        {{ service.name }} <span v-if="service.price"> {{ ` ( ${service.price} ₽ ) ` }}</span>
      </span>

      <component :is="childComponentName" :services="service.children || []" v-show="service.isToggle" />
    </li>
  </ul>
</template>

<script>
export default {
  name: 'Tree',
  props: {
    services: Array
  },
  data() {
    return {
      childComponentName: 'Tree',
      itemsList: [],
    };
  },
  methods: {
    toggle(service) {
      service.isToggle = !service.isToggle;

      this.$set(this.itemsList, this.itemsList.indexOf(service),  {...service});
    }
  },
  mounted() {
    this.itemsList = this.services
  },
  watch: {
    services(val) {
      this.itemsList = val
    }
  }
}
</script>

<style scoped>
ul {
  list-style-type: none;
  padding-left: 50px;
}

li {
  margin-bottom: 10px;

}


.service-name {
  margin-left: 10px;
  display: flex;
  align-items: center;
  height: 30px;
  font-weight: 500;
  font-size: 24px;
  margin-bottom: 10px;
}

svg {
  margin-right: 10px;
}

.service-name.pointer {
  cursor: pointer;
}

.rotate-base {

  transform: rotate(90deg);
}

.rotate {
  transform: rotate(180deg);
}</style>
