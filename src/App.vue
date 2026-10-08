<script setup>
import { ref, onUnmounted } from 'vue'
const wishes = ['随心一问', '事业学业', '姻缘人际', '财运生活']
const selected = ref('随心一问')
const phase = ref('idle')
const result = ref(null)
let timer
const fortunes = [
  { level: '上上签', luck: '大吉', title: '春风得意', poem: ['一夜春风花满枝', '前程万里正当时'], meaning: '你积攒的努力，正在悄悄开花。心中所盼，自有回响；脚下的路，也将渐渐明朗。', advice: '勇敢迈出那一步，好事正在路上。' },
  { level: '上签', luck: '吉', title: '云开见月', poem: ['浮云散尽月华明', '好景徐来伴君行'], meaning: '暂时的等待，是为了遇见更好的时机。保持你的节奏，答案会在恰好的时候到来。', advice: '慢一点也无妨，属于你的不会走远。' },
  { level: '上上签', luck: '大吉', title: '花开有时', poem: ['满园新绿迎春到', '一树繁花为你开'], meaning: '新的机缘已在身边，平凡的日子也藏着惊喜。敞开心扉，去迎接温柔而美好的相逢。', advice: '把心愿付诸行动，今天就是好时候。' },
  { level: '上签', luck: '吉', title: '顺水行舟', poem: ['轻舟顺水过千山', '一路清风一路安'], meaning: '你正走在适合自己的路上。无需急着抵达，沿途的收获与善意，会陪你稳稳向前。', advice: '守住初心，顺势而行。' },
  { level: '上上签', luck: '大吉', title: '喜鹊登枝', poem: ['喜鹊登枝传好信', '人间佳事应心来'], meaning: '好消息正在向你靠近。你给予世界的真诚，终会化作善意回到身边，所求皆有好光景。', advice: '留意身边的小惊喜，也记得分享快乐。' },
  { level: '上签', luck: '吉', title: '岁岁长安', poem: ['心有暖阳春常在', '岁岁平安福自来'], meaning: '安稳也是一种珍贵的好运。好好照顾自己，珍惜眼前的人与事，日子会越来越有滋味。', advice: '做一件让自己开心的小事。' },
]
function draw() {
  if (phase.value === 'drawing') return
  phase.value = 'drawing'
  timer = setTimeout(() => {
    result.value = { ...fortunes[Math.floor(Math.random() * fortunes.length)], wish: selected.value }
    phase.value = 'revealed'
  }, 1900)
}
function reset() { phase.value = 'idle'; result.value = null }
onUnmounted(() => clearTimeout(timer))
</script>

<template>
  <div class="site-shell">
    <header class="site-header">
      <a class="brand" href="./" aria-label="见喜首页"><span class="brand-mark">见喜</span><span class="brand-name">见 喜<small>GOOD THINGS AWAIT</small></span></a>
      <span class="header-note">一签一念 · 皆是好光景 <span class="small-sun">☀</span></span>
    </header>
    <main>
      <div class="eyebrow"><span></span> 心有期许，自有回响 <span></span></div>
      <h1>为你，求一签<span>好光景</span><i>吉</i></h1>
      <p class="intro">暂且放下匆忙，心中默念所愿。<br class="mobile-break">让这一支好签，捎来生活的温柔回信。</p>
      <section class="fortune-stage" :class="phase" aria-label="抽签区域">
        <div class="stage-top"><span>见喜签阁</span><span class="stage-number">NO. 001 — ∞</span></div>
        <template v-if="phase !== 'revealed'">
          <div class="scene">
            <div class="halo"></div><div class="halo inner"></div>
            <span class="scene-verse verse-left">心诚则灵</span><span class="scene-verse verse-right">所遇皆吉</span>
            <svg class="cloud cloud-left" viewBox="0 0 180 70" aria-hidden="true"><path d="M0 51h115c30 0 27-31 8-31-15 0-22 20-10 25M24 36h51c22 0 23-27 7-27-15 0-19 19-8 19M83 61h80"/></svg>
            <svg class="cloud cloud-right" viewBox="0 0 180 70" aria-hidden="true"><path d="M0 51h115c30 0 27-31 8-31-15 0-22 20-10 25M24 36h51c22 0 23-27 7-27-15 0-19 19-8 19M83 61h80"/></svg>
            <div class="fortune-object" :class="{ shaking: phase === 'drawing' }">
              <div class="sticks"><span v-for="n in 7" :key="n" :style="{ '--n': n }"><b>{{ ['福', '安', '喜', '吉', '顺', '好', '愿'][n - 1] }}</b></span></div>
              <div class="vessel"><div class="vessel-rim"></div><div class="vessel-lines"></div><div class="vessel-label"><span>见</span><span>喜</span><small>吉签</small></div><div class="vessel-bottom"></div></div>
            </div>
            <div class="object-shadow"></div>
          </div>
          <div class="draw-controls">
            <p class="prompt">{{ phase === 'drawing' ? '好签将至，请稍候…' : '此刻，你心中所念为何？' }}</p>
            <div class="wish-tabs" role="group" aria-label="选择心愿"><button v-for="wish in wishes" :key="wish" :class="{ active: selected === wish }" :disabled="phase === 'drawing'" @click="selected = wish">{{ wish }}</button></div>
            <button class="draw-button" :disabled="phase === 'drawing'" @click="draw"><span class="button-star">✧</span>{{ phase === 'drawing' ? '正在为你寻一支好签' : '诚心求一签' }}<span aria-hidden="true">↗</span></button>
            <p class="control-note">一份小小的仪式，一点大大的好运</p>
          </div>
        </template>
        <div v-else class="result-content" aria-live="polite">
          <div class="result-topline">{{ result.wish }} · 见喜有信</div>
          <div class="fortune-rank">{{ result.level }}<span>{{ result.luck }}</span></div>
          <h2>{{ result.title }}</h2>
          <div class="poem"><p v-for="line in result.poem" :key="line">{{ line }}</p></div>
          <div class="interpretation"><span>签意 · 为你解签</span><p>{{ result.meaning }}</p></div>
          <div class="advice">✧ {{ result.advice }}</div>
          <button class="draw-button" @click="reset">收下好运，再求一签 <span>↗</span></button>
          <p class="control-note">愿这一签，陪你从容走向好日子</p>
        </div>
        <div class="corner corner-tl"></div><div class="corner corner-tr"></div><div class="corner corner-bl"></div><div class="corner corner-br"></div>
      </section>
      <section class="below-note"><span class="note-icon">✧</span><div><h3>这里的每一签，都藏着好意。</h3><p>吉签予你宽慰，好运由你创造。愿你所行皆坦途，所遇皆温柔。</p></div></section>
    </main>
    <footer><span>见喜 · 愿你日日有喜</span><span>签文仅供娱乐，生活的答案在你手中</span><span class="footer-seal">宜<br>欢喜</span></footer>
  </div>
</template>
