<template>
  <div class="py-8">
    <div class="container max-w-screen-md p-8 mx-auto bg-white" v-if="document">
      <h1 class="text-4xl font-bold text-center">{{ document.title }} <sup class="p-1 text-sm capitalize bg-purple-400 rounded-full">{{document.status}}</sup></h1>


<div class="flex flex-col justify-between lg:flex-row">
 <div class="mt-4">
        <p class="text-sm font-bold text-green-700 uppercase">OBJECT</p>
         <p class="mt-2 tracking-wide text-gray-800">{{document.object}}</p>
      </div>



</div>



          <div class="mt-4">
        <p class="text-sm font-bold text-green-700 uppercase">REFERENCE</p>
         <p class="mt-2 tracking-wide text-gray-800">{{document.reference}}</p>
      </div>


          <div class="mt-4">
        <p class="text-sm font-bold text-green-700 uppercase">OWNER</p>
         <p class="mt-2 tracking-wide text-gray-800">{{document.expeditor.name}}</p>
      </div>




       <div class="mt-4">
        <p class="mb-4 text-sm font-bold text-green-700 uppercase">Download</p>
         <a :href="`http://localhost:7777/api/files/${document.file_id}`" target="_blank" class="px-4 py-2 text-white bg-green-700 rounded-lg hover:bg-green-500">Download</a>
      </div>

    </div>
    <div v-else>
      <h1 class="text-4xl font-bold text-center">Document not found</h1>
    </div>
  </div>
</template>

<script>
export default {
  layout: 'dashboard',
  data() {
    return {
      document: null,
      notFound: false,
    }
  },

  async fetch() {
    this.document = await this.$axios.$get(
      `/documents/${this.$route.params.id}`
    )
  },
}
</script>

<style lang="scss" scoped>
</style>
