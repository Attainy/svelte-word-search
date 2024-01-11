<script>
  // 모듈 방식
  import Main from "./pages/Main.svelte";
  import Admin from "./pages/Admin.svelte";
  import Game from "./pages/Game.svelte";
  import Router from "svelte-spa-router";
  import NotFound from "./pages/NotFound.svelte";

  const routes = {
    '/': Main,
    '/admin': Admin,
    '/game/:id': Game,
    '*': NotFound // 그 외 모든 페이지 (잘못된 경로)
  }
  
</script>

<Router routes={routes} />

<style>

</style>