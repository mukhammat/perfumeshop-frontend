<template>
    <header class="header-global fixed-top" style="top: 0;">
        <!-- Navbar default -->
        <base-nav v-for="menu in menus"
                  :key="menu.type"
                  :type="menu.type"
                  effect="dark"
                  expand
                  title="Default Color"
                  :content-id="`navbar-${menu.type}`">
            <div class="row" slot="content-header" slot-scope="{closeMenu}">
                <div class="col-6 collapse-brand">
                    <a href="./index.html">
                        <img src="img/brand/blue.png">
                    </a>
                </div>
                <div class="col-6 collapse-close">
                    <close-button @click="closeMenu" :target="`navbar-${menu.type}`">

                    </close-button>
                </div>
            </div>
            <component :is="menu.menuComponent"></component>
        </base-nav>
    </header>

</template>
<script>
import BaseNav from "@/components/BaseNav";
import CloseButton from "@/components/CloseButton";
import Menu1 from "@/views/components/Navigation/Menu1";

export default {
  components: {
    BaseNav,
    CloseButton,
    Menu1,

  },
  data() {
    return {
      menus: [
        { type: "default", menuComponent: Menu1 },
      ]
    };
  }
};
</script>
<style>
</style>
