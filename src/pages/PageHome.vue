<template>
  <q-page class="relative-position">
    <q-scroll-area class="absolute fullscreen">
      <div class="q-py-lg q-px-md row items-end q-col-gutter-sm">
      <div class="col">
        <q-input
          bottom-slots
          class="new-qweet"
          v-model="newQweetContent"
          placeholder="What's happening?"
          counter
          maxlength="280"
          autogrow
        >
          <template v-slot:before>
            <q-avatar size="xl">
              <img src="https://cdn.quasar.dev/img/avatar5.jpg" />
            </q-avatar>
          </template>
        </q-input>
      </div>
      <div class="col col-shrink">
        <q-btn
          @click="addNewQweet"
          :disable="!newQweetContent"
          class="q-mb-lg"
          unelevated
          rounded
          color="primary"
          label="Qweet"
          no-caps
        />
      </div>
      </div>

      <q-separator class="divider" size="10px" color="grey-2" />

      <q-list separator>
      <transition-group
        appear
        enter-active-class="animated fadeIn slow"
        leave-active-class="animated fadeOut slow"
      >
        <q-item 
          v-for="qweet in qweets" 
          :key="qweet.date" 
          class="qweet q-py-md"
        >
        <q-item-section avatar top>
          <q-avatar size="xl">
            <img src="https://cdn.quasar.dev/img/avatar2.jpg" />
          </q-avatar>
        </q-item-section>

        <q-item-section>
          <q-item-label class="text-subtitle1">
            <strong>Hoàng Anh</strong>
            <span class="text-grey-7"> 
              @hoang__anh
              <br class="lt-md"> &bull; {{ relativeDate(qweet.date) }}
            </span>              
          </q-item-label>
          <q-item-label class="qweet-content text-body1">
            {{ qweet.content }}
          </q-item-label>
          <div class="qweet-icon row justify-between q-mt-sm">
            <q-btn flat round color="grey" icon="far fa-comment" size="sm" />
            <q-btn flat round color="grey" icon="fas fa-retweet" size="sm" />
            <q-btn flat round color="grey" icon="far fa-heart" size="sm" />
            <q-btn 
              @click="deleteQweet(qweet)"
              flat 
              round 
              color="grey" 
              icon="fas fa-trash" 
              size="sm" 
            />
          </div>
        </q-item-section>
        </q-item>

      </transition-group>
      </q-list>
   </q-scroll-area>
  </q-page>
</template>

<script>
import { formatDistance } from "date-fns";

export default {
  name: "PageHome",
  data() {
    return {
      newQweetContent: "",
      qweets: [
        {
          content:
            "You can’t go around building a better world for people. Only people can build a better world for people. Otherwise it’s just a cage. ",
          date: 1628240242011,
        },
        {
          content:
            "There are only two ways to live your life. One is as though nothing is a miracle. The other is as though everything is a miracle.",
          date: 1628240270477,
        },
        {
          content:
            "You only live once, but if you do it right, once is enough.",
          date: 1628240281712,
        },
      ],
    };
  },
  methods: {
    relativeDate(value) {
      return formatDistance(value, new Date())
    },
    addNewQweet() {
      let newQweet = {
        content: this.newQweetContent,
        date: Date.now()
      }
      this.qweets.unshift(newQweet)
      this.newQweetContent = ''
    },
    deleteQweet(qweet){
      let dateToDelete = qweet.date
      let index = this.qweets.findIndex(qweet => qweet.date === dateToDelete)
      this.qweets.splice(index,1)
    }
  }
}
</script>

<style lang="sass">
.new-qweet
  textarea
    font-size: 19px
    line-height: 1.4em !important

.divider
  border-top: 1px solid
  border-bottom: 1px solid
  border-color: $grey-4

.qweet:not(:first-child)
  border-top: 1px solid rgba(0,0,0,0.12)

.qweet-content
  white-space: pre-line

.qweet-icon
  margin-left: -5px
</style>
