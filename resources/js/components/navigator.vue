<template>
  <nav class="flex-1 overflow-y-auto py-6 md:block hidden bg-red-500">
    <ul class="space-y-2 px-6" v-for="(baseNav, index) in navigator" :key="index">
      <p v-if="baseNav.subItems.length" class="font-bold uppercase text-gray-50 text-xs tracking-wider">
        {{ baseNav.name }}
      </p>

      <template v-if="baseNav.subItems">
        <li v-for="(nav, i) in baseNav.subItems" :key="i">
          <app-link :class="{ 'bg-red-400': isUrl(nav.url.slice(1)) }" class="focus:outline-none flex items-center gap-2 py-2 px-2 rounded-lg font-medium transition hover:bg-red-400 focus:bg-red-400 text-white" :href="nav.url">
            <icon :name="nav.heroicon" :type="nav.icon" class="w-4 h-4" />
            <span class="text-sm"> {{ nav.name }} </span>
          </app-link>
        </li>
      </template>
      <template v-if="baseNav && baseNav.subItems">
        <li v-if="baseNav.heroicon || baseNav.icon">
          <app-link :class="{ 'bg-red-400': isUrl(nav.url.slice(1)) }" class="my-3 focus:outline-none flex items-center gap-2 py-2 px-2 rounded-lg font-medium transition hover:bg-red-400 focus:bg-red-400 text-white" :href="baseNav.url">
            <icon :name="baseNav.heroicon" :type="baseNav.icon" class="w-4 h-4" />
            <span class="text-sm"> {{ baseNav.name }} </span>
          </app-link>
        </li>
      </template>

      <li class="pt-3 pb-6" v-if="baseNav.subItems.length && index != navigator.length - 1">
        <hr class="border-red-600" />
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  props: {
    navigator: Object,
  },
  methods: {
    isUrl(...urls) {
      let currentUrl = this.$page.url.substr(1);
      if (urls[0] === "") {
        return currentUrl === "";
      }

      return urls.filter((url) => currentUrl.startsWith(url)).length;
    },
  },
};
</script>
