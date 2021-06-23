<template>
  <div class="container py-8 mx-auto">
    <h2 class="mb-8 text-4xl font-bold">Received Documents</h2>
    <div class="w-full">
      <div class="my-6 bg-white rounded shadow-md">
        <table class="w-full table-auto min-w-max">
          <thead>
            <tr
              class="text-sm leading-normal text-gray-600 uppercase bg-gray-200"
            >
              <th class="px-6 py-3 text-left">Title</th>
              <!--  <th class="px-6 py-3 text-left">Owner</th> -->
              <th class="px-6 py-3 text-center">Forwarded by</th>
              <th class="px-6 py-3 text-center">Status</th>
              <th class="px-6 py-3 text-center">Last updated</th>
              <th class="px-6 py-3 text-center">action</th>
            </tr>
          </thead>
          <tbody class="text-sm font-light text-gray-600">
            <tr
              v-for="doc in myDocuments"
              :key="doc.id"
              class="border-b border-gray-200 hover:bg-gray-100"
            >
              <td class="px-6 py-3 text-left whitespace-nowrap">
                <div class="flex items-center">
                  <nuxt-link
                    :to="`/dashboard/documents/${doc.document.id}`"
                    class="font-medium"
                    >{{ doc.document.title }}</nuxt-link
                  >
                </div>
              </td>
              <!-- <td class="px-6 py-3 text-left">
                  <div class="flex items-center">
                    <div class="mr-2">
                      <img
                        class="w-6 h-6 rounded-full"
                        src="https://randomuser.me/api/portraits/men/1.jpg"
                      />
                    </div>
                    <span>{{doc.document.expeditor.name}}</span>
                  </div>
                </td> -->

              <td class="px-6 py-3 text-left">
                <div class="flex items-center">
                  <div class="mr-2">
                    <img
                      class="w-6 h-6 rounded-full"
                      src="https://randomuser.me/api/portraits/men/1.jpg"
                    />
                  </div>
                  <span>{{ doc.expeditor.name }}</span>
                </div>
              </td>
              <!--  <td class="px-6 py-3 text-center">
                  <div class="flex items-center justify-center">
                    <img
                      class="w-6 h-6 transform border border-gray-200 rounded-full hover:scale-125"
                      src="https://randomuser.me/api/portraits/men/1.jpg"
                    />
                    <img
                      class="w-6 h-6 -m-1 transform border border-gray-200 rounded-full hover:scale-125"
                      src="https://randomuser.me/api/portraits/women/2.jpg"
                    />
                    <img
                      class="w-6 h-6 -m-1 transform border border-gray-200 rounded-full hover:scale-125"
                      src="https://randomuser.me/api/portraits/men/3.jpg"
                    />
                  </div>
                </td> -->
              <td class="px-6 py-3 text-center">
                <span
                  class="px-3 py-1 text-xs text-purple-600 bg-purple-200 rounded-full "
                  >{{ doc.document.status }}</span
                >
              </td>
              <td class="px-6 py-3 text-center">
                <!--  <div class="flex justify-center item-center">
                    <div
                      class="w-4 mr-2 transform hover:text-purple-500 hover:scale-110"
                    >
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                      >
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"
                        />
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"
                        />
                      </svg>
                    </div>
                    <div
                      class="w-4 mr-2 transform hover:text-purple-500 hover:scale-110"
                    >
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                      >
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.572L16.732 3.732z"
                        />
                      </svg>
                    </div>
                    <div
                      class="w-4 mr-2 transform hover:text-purple-500 hover:scale-110"
                    >
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                      >
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
                        />
                      </svg>
                    </div>
                  </div> -->
                {{ timeAge(doc.updated_at) }}
              </td>

              <td
                class="flex flex-wrap px-6 py-3 text-center"
                v-if="
                  doc.document.status !== 'complete' &&
                  doc.document.status !== 'cancel'
                "
              >
                <button
                  v-if="doc.id === doc.document.last_share"
                  @click="forward(doc)"
                  class="px-3 py-1 text-xs text-green-600 bg-purple-200 rounded-full "
                >
                  Forward
                </button>

                <button
                  v-if="doc.id === doc.document.last_share"
                  @click="setStatus(doc.document.id, 'complete', doc)"
                  class="px-3 py-1 mx-1 text-xs text-green-600 bg-purple-200 rounded-full "
                >
                  Complete
                </button>

                <button
                  v-if="doc.id === doc.document.last_share"
                  @click="setStatus(doc.document.id, 'cancel', doc)"
                  class="px-3 py-1 text-xs text-purple-600 bg-purple-200 rounded-full "
                >
                  Cancel
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

<div v-if="shareWindow" @click="shareWindow=false" class="absolute inset-0 flex flex-col justify-center ">
  <div class="absolute inset-0 z-0 bg-gray-800 opacity-25">

  </div>
  <div class="relative z-10 max-w-2xl p-8 mx-auto bg-white shadow-xl" @click.stop>
      <p class="text-xl font-bold text-center">Forward to :</p>
      <select
                      class="block w-full px-4 py-3 border rounded appearance-none bg-grey-lighter text-grey-darker border-grey-lighter"
                      id="grid-last-name"
                      type="text"
                      placeholder="Doe"
                      v-model.number="doc.receiver_id"
                    >
                      <option
                        v-for="user in users"
                        :key="user.id"
                        :value="user.id"
                      >
                        {{ user.name }}
                      </option>
                    </select>

                    <div class="flex justify-center">
                      <button @click.stop="sendDoc" class="p-4 py-2 mt-4 text-white bg-green-700 rounded-lg hover:bg-green-500">Send</button>
                    </div>
    </div>
</div>

  </div>
</template>

<script>
import { timeAgo } from '../../utilities/timeago'
export default {
  layout: 'dashboard',

  data() {
    return {
      myDocuments: [],
      doc:{
        receiver_id:'',
        share_id:''
      },
      shareWindow: false,
    }
  },
  async fetch() {
    this.myDocuments = await this.$axios.$get('documents/received')
     this.users = await this.$axios.$get('/users')
    if (this.users) {
      this.doc.receiver_id = this.users[0].id
    }
  },
  methods: {
    timeAge(date) {
     return(timeAgo(date))

    },
    async setStatus(id, status, doc) {
      await this.$axios.$put(`/documents/${id}/${status}`)
      doc.status = status
      this.$fetch()
    },

    forward(doc) {
      this.doc.share_id = doc.id;
      this.shareWindow = true;
    },

    async sendDoc(){
      await this.$axios.$post("/documents/forward", this.doc)
      this.$fetch()
      this.shareWindow = false
    }
  },
}
</script>

<style lang="scss" scoped>
</style>
