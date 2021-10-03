<template>
  <div>
    <button @click="execAsyncFunc">プリフェッチ後非同期処理の後に遷移</button>
    <h1>まだプリフェッチしない</h1>
    <h1>まだプリフェッチしない</h1>
    <h1>まだプリフェッチしない</h1>
    <h1>まだプリフェッチしない</h1>
    <h1>まだプリフェッチしない</h1>
    <h1>まだプリフェッチしない</h1>
    <h1>まだプリフェッチしない</h1>
    <h1>まだプリフェッチしない</h1>
    <h1>まだプリフェッチしない</h1>
    <h1>まだプリフェッチしない</h1>
    <h1>まだプリフェッチしない</h1>
    <h1>まだプリフェッチしない</h1>
    <h1>まだプリフェッチしない</h1>
    <h1>まだプリフェッチしない</h1>
    <button ref="prefetchTarget" @click="runSomeThenRouterPush">
      画面内に表示されたらプリフェッチ！
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from '@nuxtjs/composition-api'
import { usePrefetchRouter } from '~/modules/usePrefetchRouter'

export default defineComponent({
  setup() {
    // プリフェッチしたいpathを渡してuseする。
    const {
      prefetchRouter,
      router,
      prefetchTarget,
      goToPage,
      prefetch,
      prefetchAll,
    } = usePrefetchRouter('/hoge')
    // ユースケース①：ページcreatedから一定時間後にプリフェッチ
    setTimeout(() => {
      prefetch()
    }, 5000)

    // ユースケース②：プリフェッチして非同期コールバック後にページ遷移
    const execAsyncFunc = () =>
      prefetchRouter(async () => {
        console.log('非同期処理の前にprefetch')
        await fetch('https://jsonplaceholder.typicode.com/todos/1')
          .then((response) => response.json()) // この後にページ遷移
          .catch((e) => {
            console.log(e)
          })
      })
    // ユースケース③：選択したpathを全てプリフェッチ
    prefetchAll(['/hoge', '/fuga'])

    // オプション①：ページ遷移
    const runSomeThenRouterPush = () => {
      console.log('なんかの処理')
      goToPage() // router.push('/test')
    }
    // オプション②：routerも使える
    const runSomeThenRouterPush2 = () => {
      console.log('重い処理・非同期処理')
      router.push('/hoge')
    }

    return {
      prefetchTarget,
      execAsyncFunc,
      runSomeThenRouterPush,
      runSomeThenRouterPush2,
    }
  },
})
</script>
