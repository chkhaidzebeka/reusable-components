<script setup lang="ts">
/**
 * @note adds extra active class if queries match as well
 */

import { computed, useAttrs } from "vue";
import { RouteLocationRaw } from "vue-router";
import { useRoute, useRouter } from "vue-router";

const props = withDefaults(
  defineProps<{
    watchableQueries?: string[];
    queryExactActiveClassName?: string;
  }>(),
  {
    watchableQueries: () => ["category"],
    queryExactActiveClassName: "query-exact-active",
  }
);

const route = useRoute();
const router = useRouter();
const attrs = useAttrs();

const filterQuery = (query: object) => {
  const initialValue: Record<string, unknown> = {};

  return Object.entries(query).reduce((acc, [key, value]) => {
    if (props.watchableQueries.includes(key)) {
      acc[key] = value;
    }
    return acc;
  }, initialValue);
};

const isExactActive = computed(() => {
  const routeQuery = route.query;
  const currentRoutePath = route.path;

  const currentLink = router.resolve(attrs.to as RouteLocationRaw);
  const { query: linkQuery = {}, path: linkRoutePath } = currentLink;

  const filteredRouteQuery = filterQuery(routeQuery);
  const filteredLinkQuery = filterQuery(linkQuery);

  const queryMatches =
    JSON.stringify(filteredRouteQuery) === JSON.stringify(filteredLinkQuery);

  const routeMatches = currentRoutePath === linkRoutePath;
  const isExactMatch = routeMatches && queryMatches;

  return isExactMatch;
});
</script>

<template>
  <router-link
    :class="['v-link', { [queryExactActiveClassName]: isExactActive }]"
    v-bind="{ ...$attrs }"
  >
    <slot />
  </router-link>
</template>
