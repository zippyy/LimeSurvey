<script>
import _ from 'lodash';
import ajaxMethods from '../../mixins/runAjax.js';
import Menuicon from './_menuicon.vue';
import Submenu from './_submenu.vue';

export default {
    name: 'sidemenu',
    components : {
        'menuicon' : Menuicon,
        'submenu': Submenu
    },
    mixins: [ajaxMethods],
    props: {
        'openSubpanelId' : {type: Number},
    },
    data(){
        return {
            menues : {},
        };
    },
    computed: {
        sortedMenues(){
            return _.orderBy(this.$store.state.sidemenus,(a)=>{return parseInt((a.ordering || 999999)) }, ['asc']);
        }
    },
    methods:{
        sortedMenuEntries(entries) {
            const self = this;
            let orderedArray = _.orderBy(entries,(a)=>{return parseInt((a.ordering || 999999)) }, ['asc']);            
            return orderedArray;
        },
        setActiveMenuIndex(menuItem){
            let activeMenuIndex = menuItem.id;
            this.$store.commit('lastMenuOpen', menuItem)
            
        },
        setActiveMenuItemIndex(menuItem){
            let activeMenuIndex = menuItem.id;
            this.$store.commit('lastMenuItemOpen', menuItem)
            
        },
        setOpenSubpanel(sId){
            this.openSubpanelId = sId;
            this.$emit('menuselected', sId);
        },
        debugOut(obj){
            return JSON.stringify(obj);
        }
    },
    created(){
        const self = this;
        //first load old settings from localStorage
        
    },
    mounted(){
        const self = this;
        this.updatePjaxLinks();

        $(document).on('vue-reload-remote', ()=>{
            this.$forceUpdate();
        });
    }
}
</script>
<template>
    <div class="ls-flex-column fill menu-pane overflow-enabled ls-space padding all-0 margin top-5" >
        <div  v-for="menu in sortedMenues" :title="menu.title" :id="menu.id" class="ls-flex-row ls-space padding all-0" v-bind:key="menu.id">
            <submenu :menu="menu"></submenu>
        </div>
    </div>
</template>
<style lang="scss">

</style>
