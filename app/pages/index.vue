<script setup lang="ts">
const { data: page } = await useAsyncData("index", () =>
  queryCollection("index").first()
);

const title = page.value?.seo?.title || page.value?.title;
const description = page.value?.seo?.description || page.value?.description;

useSeoMeta({
  titleTemplate: "",
  title,
  ogTitle: title,
  description,
  ogDescription: description,
});
</script>

<template>
  <div v-if="page">
    <UPageHero
      :title="page.title"
      :description="page.description"
      :links="page.hero.links"
      orientation="horizontal"
    >
      <template #top>
        <div
          class="absolute rounded-full dark:bg-primary blur-[300px] size-60 sm:size-80 transform -translate-x-1/2 left-1/2 -translate-y-80"
        />

        <LazyStarsBg />
      </template>

      <!-- <PromotionalVideo /> -->
      <img
        src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fi.pinimg.com%2Foriginals%2F12%2F2a%2Fcb%2F122acbe6ec65e27b02e5e5475a27872c.jpg&f=1&nofb=1&ipt=d7905b33b0235000d00e7ce881948869d1bd2faf970ad154b506a7fc4d71111f"
        alt="App screenshot"
        class="rounded-lg shadow-2xl ring ring-default"
      />
    </UPageHero>

    <UPageSection
      v-for="(section, index) in page.sections"
      :key="index"
      :title="section.title"
      :description="section.description"
      :orientation="section.orientation"
      :reverse="section.reverse"
      :features="section.features"
    >
      <ImagePlaceholder />
    </UPageSection>

    <UPageSection
      :title="page.features.title"
      :description="page.features.description"
    >
      <UPageGrid>
        <UPageCard
          v-for="(item, index) in page.features.items"
          :key="index"
          v-bind="item"
          spotlight
        />
      </UPageGrid>
    </UPageSection>

    <UPageSection
      id="testimonials"
      :headline="page.testimonials.headline"
      :title="page.testimonials.title"
      :description="page.testimonials.description"
    >
      <UPageColumns class="xl:columns-4">
        <UPageCard
          v-for="(testimonial, index) in page.testimonials.items"
          :key="index"
          variant="subtle"
          :description="testimonial.quote"
          :ui="{
            description:
              'before:content-[open-quote] after:content-[close-quote]',
          }"
        >
          <template #footer>
            <UUser v-bind="testimonial.user" size="lg" />
          </template>
        </UPageCard>
      </UPageColumns>
    </UPageSection>

    <USeparator />

    <UPageCTA v-bind="page.cta" variant="naked" class="overflow-hidden">
      <div
        class="absolute rounded-full dark:bg-primary blur-[250px] size-40 sm:size-50 transform -translate-x-1/2 left-1/2 -translate-y-80"
      />

      <LazyStarsBg />
    </UPageCTA>
  </div>
</template>
