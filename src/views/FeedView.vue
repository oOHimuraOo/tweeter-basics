<script setup lang="ts">
import { fakeTweetsDb } from '@/utils/mocks'
import PostTweet from '../components/PostTweet.vue'
import { computed, onMounted, reactive } from 'vue'
import TweetClass from '@/utils/models/tweets'

const estado = reactive<{ tweets: TweetClass[] }>({
  tweets: [],
})

function coletarTweets() {
  const novos_tweets = fakeTweetsDb.filter(
    item => !estado.tweets.some(tweet => tweet.id === item.id),
  )
  novos_tweets.forEach(tweet => {
    estado.tweets.push(tweet)
  })
}

function handleTweetSubmited() {
  coletarTweets()
}

onMounted(() => {
  coletarTweets()
  setInterval(() => {
    coletarTweets()
  }, 10000)
})

const tweetsRevertidos = computed(() => {
  return [...estado.tweets].reverse()
})
</script>

<template>
  <div class="container">
    <div class="post_container">
      <PostTweet @tweet-submited="handleTweetSubmited" />
    </div>
    <div class="tweets_container">
      <ul>
        <span>feed</span>
        <li v-for="tweet in tweetsRevertidos" :key="tweet.id">
          <div>
            <img :src="tweet.img" alt="perfil" />
            <h3>{{ tweet.name }}</h3>
          </div>
          <p>
            {{ tweet.post }}
          </p>
          <p class="data_hora">data:{{ tweet.data }} hora:{{ tweet.hour }}</p>
        </li>
      </ul>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@use '@/assets/Variaveis.scss';

.post_container {
  margin: 32px 0;
}

.tweets_container {
  padding: 16px;
  border: 2px solid Variaveis.$corTerciaria;
  border-radius: 8px;
  background-color: Variaveis.$corDeFundoQuaternaria;
  ul {
    display: flex;
    flex-direction: column;

    span {
      margin-bottom: 32px;
      font-size: Variaveis.$fontSizeM;
      color: Variaveis.$corSecundaria;
      font-weight: bold;
      text-transform: capitalize;
    }

    li {
      border: 2px solid Variaveis.$corTerciaria;
      border-radius: 8px;
      margin-bottom: 8px;
      padding: 8px;
      background-color: Variaveis.$corDeFundoPrimaria;
      div {
        display: flex;
        align-items: center;
        margin-bottom: 8px;

        img {
          max-height: 30px;
          max-width: 30px;
          border: 2px solid Variaveis.$corTerciaria;
          border-radius: 50px;
          height: 100%;
          width: 100%;
          margin-right: 8px;
        }

        h3 {
          color: Variaveis.$corSecundaria;
          font-size: Variaveis.$fontSizeM;
          text-transform: capitalize;
        }
      }

      p {
        font-size: Variaveis.$fontSizePP;
        color: Variaveis.$corSecundaria;
        overflow-wrap: break-word;
      }

      .data_hora {
        display: flex;
        justify-content: end;
        text-align: right;
        font-size: Variaveis.$fontSizePPP;
        color: Variaveis.$corTerciaria;
      }
    }
  }
}
</style>
