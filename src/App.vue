<template>
      <Menubar :model="items">
          <template #start>
              <img alt="logo" src="./assets/logo.png" height="40" class="mr-2" />
          </template>
          <template #item="{ label, item, props, root, hasSubmenu }">
              <router-link v-if="item.route" v-slot="routerProps" :to="item.route" custom>
                  <a :href="routerProps.href" v-bind="props.action">
                      <span v-bind="props.icon" />
                      <span v-bind="props.label">{{ label }}</span>
                  </a>
              </router-link>
              <a v-else :href="item.url" :target="item.target" v-bind="props.action">
                  <span v-bind="props.icon" />
                  <span v-bind="props.label">{{ label }}</span>
                  <span :class="[hasSubmenu && (root ? 'pi pi-fw pi-angle-down' : 'pi pi-fw pi-angle-right')]" v-bind="props.submenuicon" />
              </a>
          </template>
          <template #end>
              <!-- <InputText placeholder="Search" type="text" /> -->
          </template>
      </Menubar> 

  <router-view/>
</template>

<script>
export default {
  data(){
    return{
      items: [
    
        {
            label: 'Главная',
            icon: '',
            route: '/'
        },
        {
            label: 'Список идей',
            icon: 'pi pi-star-fill',
            route: '/idea/all/list'
        },
        {
            label: 'Создать',
            icon: 'pi pi-palette',
            route: '/edit'
        }
    ]
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
