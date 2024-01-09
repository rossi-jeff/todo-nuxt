<template>
  <main id="main">
    <div id="top-wrapper">
      <TopBar />
    </div>
    <div id="scroll-body">
      <slot />
    </div>
    <div id="bottom-wrapper">
      <BottomBar />
    </div>
  </main>
</template>

<style>
#main {
  @apply flex flex-col h-screen max-h-screen p-0 m-0 bg-yellow-100 text-black;
}
#scroll-body {
  @apply flex-grow overflow-y-auto px-4;
}
#top-wrapper,
#bottom-wrapper {
  @apply h-12 p-0 m-0;
}
h1 {
  @apply text-2xl font-bold text-blue-900;
}
h2 {
  @apply text-xl font-bold text-blue-700;
}
a {
  @apply font-bold text-blue-600;
}
a:hover {
  @apply underline;
}
button {
  @apply border border-black bg-white rounded text-sm py-1 px-2;
}
button:hover {
  @apply bg-slate-200;
}
</style>
