<template>
  <div id="app">
    <h1>Дерево услуг</h1>
    <Tree :services="tree" />
  </div>
</template>

<script>
import Tree from './components/Tree.vue';
import api from './api'
export default {
  components: {
    Tree
  },
  mounted() {
    this.loadServices();
  },
  data() {
    return {
      services: []
    }
  },
  computed: {
    tree() {
        const rootNode = { id: null, children: [] };

        const nodesById = {};
        this.services.forEach(node => nodesById[node.id] = node);

        this.services.forEach(node => {
          if (node.head === null) {
            rootNode.children.push(node);
          } else {
            const parentNode = nodesById[node.head];
            if (!parentNode.children) {
              parentNode.children = [];
            }
            parentNode.children.push(node);
          }
        });

        function addChildren(node) {
          if (node.children) {
            node.children.forEach(child => {
              addChildren(child);
            });
          }
        }
        rootNode.children.forEach(node => addChildren(node));

        return rootNode.children;
    }
  },

  methods: {
    getRootServices() {
      return this.services.filter(service => service.head === null);
    },
    loadServices() {
      api.fakeResponse.takeServices().then(response => {

        this.services = response.sort((a, b) => a.sorthead - b.sorthead).map(item => Object.assign( item, { isToggle: false }));;

      })
        .catch(error => {
          console.error(error);
        });
    },
  }
}
</script>