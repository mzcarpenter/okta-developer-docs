<template>
    <li :class="{'subnav': link.subLinks}">
        <div class="link-wrap">
            <svg viewBox="0 0 320 512" v-if="link.subLinks && !(imActive || iHaveChildrenActive)">
                <path d="M0 384.662V127.338c0-17.818 21.543-26.741 34.142-14.142l128.662 128.662c7.81 7.81 7.81 20.474 0 28.284L34.142 398.804C21.543 411.404 0 402.48 0 384.662z"/>
            </svg>

            <svg viewBox="0 0 320 512" v-if="link.subLinks && (imActive || iHaveChildrenActive)">
                <path d="M31.3 192h257.3c17.8 0 26.7 21.5 14.1 34.1L174.1 354.8c-7.8 7.8-20.5 7.8-28.3 0L17.2 226.1C4.6 213.5 13.5 192 31.3 192z"/>
            </svg>

            <div v-if="link.path">
                <router-link :to="link.path" @click="setData" :class="{'router-link-active': imActive, 'router-link-exact-active': imActive}">{{link.title}}</router-link>
            </div>
            <div v-else>
                <div class="is-link" @click="iHaveChildrenActive = !iHaveChildrenActive">{{link.title}}</div>
            </div>
        </div>

        <ul v-if="link.subLinks" class="sections" v-show="iHaveChildrenActive || imActive">
            <SidebarItem v-for="(sublink, key) in link.subLinks" :key="key" :link="sublink" />
        </ul>
      </li>
</template>

<script>
export default {
    name: 'SidebarItem',
    props: ['link'],
    components: {
      SidebarItem: () => import('../components/SidebarItem.vue'),
    },
    data() {
        return {
            imActive: false,
            iHaveChildrenActive: false
        }
    },
    mounted() {
        this.setData();
    },
    watch: {
        '$route': function() {
            this.setData();
        }
    },
    methods: {
        setData: function() {
            this.imActive = false;
            if(this.link.path) {
                this.imActive = this.$page.regularPath == this.link.path;
            }
            if(this.link.activeCheck) {
                this.imActive = this.$page.regularPath.includes(this.link.activeCheck);
            }
            this.$nextTick(() => {
                this.iHaveChildrenActive = (this.$children || [] ).some((child) => {
                    return child.imActive
                });
            });
        },
        setSubnavOpen: function (link) {
        if(this.subnavOpen == null || this.subnavOpen != link) {
          this.subnavOpen = link
          return
        }
        if(this.subnavOpen == link) {
          this.subnavOpen = null
          return
        }
      }
    }
}
</script>