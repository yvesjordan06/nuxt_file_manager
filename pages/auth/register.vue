<template>
  <div>
    <div class="container max-w-full mx-auto py-24 px-6">
      <div class="font-sans">
        <div class="max-w-sm mx-auto px-6">
          <div class="relative flex flex-wrap">
            <div class="w-full relative">
              <div class="mt-6">
                <div class="text-center font-semibold text-black">
                  Doc authentication
                </div>

                <form class="mt-8">
                  <div class="mx-auto max-w-lg">
                    <div class="py-2">
                      <span class="px-1 text-sm text-gray-600">Name</span>
                      <input
                        placeholder=""
                        type="text"
                        class="
                          text-md
                          block
                          px-3
                          py-2
                          rounded-lg
                          w-full
                          bg-white
                          border-2 border-gray-300
                          placeholder-gray-600
                          shadow-md
                          focus:placeholder-gray-500
                          focus:bg-white
                          focus:border-gray-600
                          focus:outline-none
                        "
                      />
                    </div>
                    <div class="py-2">
                      <span class="px-1 text-sm text-gray-600">Email</span>
                      <input
                        placeholder=""
                        type="email"
                        class="
                          text-md
                          block
                          px-3
                          py-2
                          rounded-lg
                          w-full
                          bg-white
                          border-2 border-gray-300
                          placeholder-gray-600
                          shadow-md
                          focus:placeholder-gray-500
                          focus:bg-white
                          focus:border-gray-600
                          focus:outline-none
                        "
                      />
                    </div>
                    <div class="py-2">
                      <span class="px-1 text-sm text-gray-600">Password</span>
                      <div class="relative">
                        <input
                          placeholder=""
                          :type="show ? 'password' : 'text'"
                          class="
                            text-md
                            block
                            px-3
                            py-2
                            rounded-lg
                            w-full
                            bg-white
                            border-2 border-gray-300
                            placeholder-gray-600
                            shadow-md
                            focus:placeholder-gray-500
                            focus:bg-white
                            focus:border-gray-600
                            focus:outline-none
                          "
                        />
                        <div
                          class="
                            absolute
                            inset-y-0
                            right-0
                            pr-3
                            flex
                            items-center
                            text-sm
                            leading-5
                          "
                        >
                          <i
                            class="fa fa-eye h-6 text-lg text-gray-700"
                            aria-hidden="true"
                            v-if="show"
                            @click="show = !show"
                          ></i>
                          <i
                            class="fa fa-eye-slash h-6 text-lg text-gray-700"
                            aria-hidden="true"
                            v-else
                            @click="show = !show"
                          ></i>
                        </div>
                      </div>
                    </div>
                    <div class="flex justify-between">
                      <label class="block text-gray-500 font-bold my-4"
                        ><input
                          type="checkbox"
                          class="leading-loose text-pink-600"
                        />
                        <span class="py-2 text-sm text-gray-600 leading-snug">
                          Remember Me
                        </span></label
                      >
                      <label class="block text-gray-500 font-bold my-4"
                        ><a
                          href="#"
                          class="
                            cursor-pointer
                            tracking-tighter
                            text-black
                            border-b-2 border-gray-200
                            hover:border-gray-400
                          "
                          ><span>Forgot Password?</span></a
                        ></label
                      >
                    </div>
                    <button
                      class="
                        mt-3
                        text-lg
                        font-semibold
                        bg-green-700
                        w-full
                        text-white
                        rounded-lg
                        px-6
                        py-3
                        block
                        shadow-xl
                        hover:text-white
                        hover:bg-green-500
                      "
                    >
                      Login
                    </button>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  auth: 'guest',
  data() {
    return {
      show: true,
    }
  },
}
</script>

<style lang="scss" scoped>
</style>
