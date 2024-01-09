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
  @apply flex flex-col h-screen max-h-screen p-0 m-0;
}
#scroll-body {
  @apply flex-grow overflow-y-auto px-4;
}
#top-wrapper,
#bottom-wrapper {
  @apply h-12 p-0 m-0;
}
h1 {
  @apply text-2xl font-bold;
}
h2 {
  @apply text-xl font-bold;
}
button {
  @apply border border-black rounded text-sm py-1 px-2;
}
button:hover {
  @apply bg-slate-200;
}
</style>
