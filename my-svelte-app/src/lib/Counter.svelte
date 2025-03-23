<script lang="ts">
  import { blur, scale, slide } from "svelte/transition";

  let counter:number = (localStorage.getItem('counter')) ? Number(localStorage.getItem('counter')) : 0;

  // аудио
  let audioOnClick: HTMLAudioElement | null = null;
  let audiomusic: HTMLAudioElement | null = null;
  let audiolevel: HTMLAudioElement | null = null;
  let audioerror: HTMLAudioElement | null = null;

  // правда или лож
  let clickCooldown = false;
  let showWarning = false;
  let showButton = true;
  let timeoutStarted = false;
  let levelUpdated = false;

  // Числа
  let lastClickTime: number = 0;
  let isCards1: number = 0;
  let isCards2: number = 0;
  let isCards3: number = 0;
  let isDeer: number = 0;
  let isSvelte: number = 1;

  let level: number = (localStorage.getItem('level')) ? Number(localStorage.getItem('level')) : 0;
  let progressing: number = (localStorage.getItem('progressing')) ? Number(localStorage.getItem('progressing')) : 0;
  let hideapp = 0;
  let inventorycard: number =  0;
  let inventoryBun1: number = localStorage.getItem('inventoryBun1') ? Number(localStorage.getItem('inventoryBun1')) : 0;
  let Achievementscard: number = 0;
  let storedAchievements = localStorage.getItem('achievements1');
  let achievements1: number = storedAchievements ? Number(storedAchievements) : 0;  let clear: number = 0;
  let clearcard: number = 0;
  let menugame: number = 1;
  let bunremove: number = localStorage.getItem('bunremove') ? Number(localStorage.getItem('bunremove')) : 0;

  function localStorageSave() {
    localStorage.setItem('level', level.toString());
    localStorage.setItem('inventoryBun1', inventoryBun1.toString());
    localStorage.setItem('counter', counter.toString());
    localStorage.setItem('achievements1', achievements1.toString());
    localStorage.setItem('progressing', progressing.toString());
    localStorage.setItem('bunremove', bunremove.toString());
  }

  function localStorageMenu() {
    if (clear >= 0) {
      clearcard = 1;
      menugame = 0;
    }
  }

  function localStorageClear() {
    clearcard = 0;
    localStorage.clear();
    location.reload();
  }

  function localStorageNoClear() {
    clearcard = 0;
    menugame = 1;
  }

  function localStorageNone() {
    if (progressing >= 0) {
      bunremove = 1;
    }
  }

  function progress() {
    if (progressing >= 1){
      progressing = 0;
      level++;
      localStorageSave() 
      localStorageNone()
    }
  }

  function levels() {
    if (level >= 0){
      progressing -= 0.03;
    }
    if (level == 3 && !levelUpdated) { // Выполнится ТОЛЬКО ОДИН РАЗ
      isDeer = 1;
      inventoryBun1 = 1;
      isSvelte = 0;
      levelUpdated = true; // Блокируем повторное выполнение
      console.log("isDeer", isDeer);
    }

    audioplay()
  }

  function MainAchievements() {
    if (Achievementscard >= 0) {
      hideapp = 0;
      achievements1 = 1;
    }
  }

  function menugamesbun() {
    if (menugame >= 0) {
      hideapp = 0;
      menugame = 1;
    }
  }

  function game() {
    if (menugame >= 1) {
      hideapp = 1;
      menugame = 0;
    }
  }
  
  function InventoryBun1() {
    if (isDeer >= 0) {
      isSvelte = 0;
      isDeer = 1;
    }
  }

  function InventoryBun2() {
    if (isSvelte >= 0) {
      isDeer = 0;
      isSvelte = 1;
    }
  }

  function InventoryBack() {
    if (inventorycard >= 1) {
      hideapp = 1;
      inventorycard = 0;
    }
  }

  function MainInventory() {
    if (inventorycard >= 0) {
      hideapp = 0;
      inventorycard = 1;
    }
  }

  // Функция для увеличения счётчика
  function add() {
    counter++;
    progressing += 0.1;
    localStorageSave()
    levels()
    progress();
    count();
  }

  // Функция воспроизведения аудио
  function audioplay() {
    if (audioOnClick) {
      audioOnClick.currentTime = 0;
      audioOnClick.volume = 0.1;
      audioOnClick.play().catch((error) => {
        console.error('Ошибка воспроизведения: ', error);
      });
    }

    if (progressing >= 1 && audiolevel) {
      if (audiolevel) {
        audiolevel.currentTime = 0;
        audiolevel.volume = 0.2;
        audiolevel.play().catch((error) => {
          console.error('Ошибка воспроизведения: ', error);
        });
      }
    }

    if (showWarning >= true && audioerror) {
      if (audioerror) {
        audioerror.currentTime = 0;
        audioerror.volume = 0.2;
        audioerror.play().catch((error) => {
          console.error('Ошибка воспроизведения: ', error);
        });
      }
    }
  }

  function count() {
    if (counter == 10 && !timeoutStarted){
      isCards1 = 1;
      timeoutStarted = true;

      setTimeout(() => {
        isCards1 = 0;
        timeoutStarted = false;
      }, 1000);
    }

    if (counter == 20 && !timeoutStarted){
      isCards2 = 1;
      timeoutStarted = true;

      setTimeout(() => {
        isCards2 = 0;
        timeoutStarted = false;
      }, 1000);
    }

    if (counter == 30 && !timeoutStarted){
      isCards3 = 1;
      timeoutStarted = true;

      setTimeout(() => {
        isCards3 = 0;
        timeoutStarted = false;
      }, 1000);
    }
  }

  function main() {
    const currentTime = Date.now();
    const clickInterval = currentTime - lastClickTime;

    // Устанавливаем порог быстрого клика, чтобы можно было кликать быстрее
    const clickThreshold = 100;  // 100 мс между кликами — это уже достаточно быстро

    // Если клик был слишком быстрым, показываем предупреждение и блокируем дальнейшие клики
    if (clickInterval < clickThreshold && !clickCooldown) {
        showWarning = true;
        clickCooldown = true;
        showButton = false;
        setTimeout(() => {
            showWarning = false;
            clickCooldown = false;  
            setTimeout(() => {
                showButton = true; 
            }, 1000); // 1 секунды задержки
        }, 3000); // 3 секунд кулдаун
        return;
    } else {
        audioplay();
        add();
        localStorageSave()
        lastClickTime = currentTime;
    }
  }

  // Функция для обработки клика с ограничением
  function mainbun() {
    main();
  }
</script>

<audio bind:this={audioOnClick}>
  <source src="/audio/OnClick.mp3" type="audio/mpeg">
</audio>

<audio bind:this={audiolevel}>
  <source src="/audio/LevelUp.mp3" type="audio/mpeg">
</audio>

<audio bind:this={audioerror}>
  <source src="/audio/Error.mp3" type="audio/mpeg">
</audio>

{#if menugame}

<div id="menu" transition:slide>
  <button id="menubun" onclick={game} transition:slide>Играть</button>
  <!-- <button id="menubun" onclick={MainAchievements} transition:slide>Достижения</button> -->
  <!-- <button id="inventorybun" onclick={MainInventory} transition:slide>Фон</button> -->
  {#if bunremove}<button id="removebun" onclick={localStorageMenu} transition:slide>Удалить  прогресс</button>{/if}
</div>
{/if}

{#if clearcard}
<div id="clear" transition:slide>
  <h3>Ты дествительно хочешь удалить прогресс?</h3>
  <button id="removebun" onclick={localStorageClear} transition:slide>да</button>
  <button id="noremovebun" onclick={localStorageNoClear} transition:slide>нет</button>
</div>
{/if}

{#if hideapp}
<div id="menu" transition:slide>
  <button id="menubun" onclick={menugamesbun} transition:slide>Меню</button>
  <button id="menubun" onclick={MainInventory} transition:slide>Инвентарь</button>
</div>
<div id="progress" transition:slide>
  <h2 id="h2">Level {level}</h2>
  <progress id="prog" value={progressing} max="1" transition:slide></progress>
</div>

<div id="card" transition:slide>
  <h1 id="count" transition:slide>{counter}</h1>

  {#if showButton}
    {#if isDeer}
      <img src="/img/photo_2025-03-20_19-58-21-removebg-preview.png" id="img" transition:slide/>
    {/if}
    {#if isSvelte}
      <img src="/img/images-removebg-preview.png" id="img" transition:slide/>
    {/if}
    <button id="bun" onclick={mainbun} transition:slide>Кликай</button>
    <br>
  {/if}
</div>
{/if}

{#if inventorycard}
<div id="inventory" transition:slide>
  <button id="inventorybunBack" onclick={InventoryBack} transition:slide>Назад</button>
  <h1>Inventory</h1>
  <button id="inventorybun" onclick={InventoryBun2} transition:slide><img src="/img/images-removebg-preview.png" id="inventoryimg"/></button>
  {#if inventoryBun1}
  <button id="inventorybun" onclick={InventoryBun1} transition:slide>
    <img src="/img/photo_2025-03-20_19-58-21-removebg-preview.png" id="inventoryimg"/>
  </button>
  {/if}
</div>
{/if}

{#if showWarning}
  <div id="warning" transition:slide>
    <h3>Ти кликаєш дуже швидко! Почекай 3 секунд...</h3>
    <h3>Ты кликаешь слишком быстро! Подожди 3 секунд...</h3>
  </div>
{/if}

{#if isCards1}
  <div id="cards" transition:slide>
    <h2 id="h1">Hello</h2>
  </div>
{/if}

{#if isCards2}
  <div id="cards" transition:slide>
    <h2 id="h1">Hello World ,_,</h2>
  </div>
{/if}

{#if isCards3}
  <div id="cards" transition:slide>
    <h2 id="h1">YIPPEE</h2>
    <audio bind:this={audiomusic} autoplay volume={0.1}>
      <source src="/audio/yippee.mp3" type="audio/mpeg">
    </audio>
  </div>
{/if}