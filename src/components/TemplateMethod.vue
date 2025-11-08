<template>
  <div class="chuseok-container">
    <header class="header">
      <h1>풍요로운 한가위</h1>
    </header>

    <Transition name="fade" mode="out-in">
      <main class="main-content" v-if="activeView === 'main'" key="main">
        <div class="moon-wrapper">
          <div class="moon"></div>
          <div class="moon-shadow"></div>
        </div>
        <p class="greeting">{{ greetingMessage }}</p>

        <button @click="makeAWish" class="wish-button">소원 빌기</button>

        <div class="nav-buttons">
          <button @click="activeView = 'food'" class="nav-button">
            한가위 음식
          </button>
          <button @click="activeView = 'game'" class="nav-button">
            전통 풍습
          </button>
          <button @click="activeView = 'story'" class="nav-button">
            추석 이야기
          </button>
        </div>
      </main>

      <section
        class="content-section"
        v-else-if="activeView === 'food' && foodData"
        key="food"
      >
        <h2>{{ foodData.title }}</h2>
        <div class="card-grid">
          <div class="info-card" v-for="card in foodData.cards" :key="card.id">
            <img
              :src="card.imagePath"
              :alt="card.imageAlt"
              class="card-img"
              @error="onImgError"
            />
            <h3>{{ card.title }}</h3>
            <p>{{ card.description }}</p>
          </div>
        </div>
        <button @click="activeView = 'main'" class="back-button">
          돌아가기
        </button>
      </section>

      <section
        class="content-section"
        v-else-if="activeView === 'game' && customData"
        key="game"
      >
        <h2>{{ customData.title }}</h2>
        <div class="card-grid">
          <div
            class="info-card"
            v-for="card in customData.cards"
            :key="card.id"
          >
            <img
              :src="card.imagePath"
              :alt="card.imageAlt"
              class="card-img"
              @error="onImgError"
            />
            <h3>{{ card.title }}</h3>
            <p>{{ card.description }}</p>
          </div>
        </div>
        <button @click="activeView = 'main'" class="back-button">
          돌아가기
        </button>
      </section>

      <section
        class="content-section"
        v-else-if="activeView === 'story' && storyData"
        key="story"
      >
        <h2>{{ storyData.title }}</h2>
        <div class="card-grid">
          <div class="info-card" v-for="card in storyData.cards" :key="card.id">
            <img
              :src="card.imagePath"
              :alt="card.imageAlt"
              class="card-img"
              @error="onImgError"
            />
            <h3>{{ card.title }}</h3>
            <p>{{ card.description }}</p>
          </div>
        </div>
        <button @click="activeView = 'main'" class="back-button">
          돌아가기
        </button>
      </section>
    </Transition>

    <footer class="footer">
      <p>가족과 함께 즐겁고 넉넉한 시간 보내세요.</p>
      <p style="font-size: 0.9rem; color: #8080a0; margin-top: 0.5rem">
        ✨ Template Method 패턴 적용: 섹션 렌더링의 알고리즘 골격을 정의
      </p>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";

interface CardData {
  id: string;
  title: string;
  description: string;
  imagePath: string;
  imageAlt: string;
}

abstract class CardSection {
  public render(): {
    title: string;
    cards: CardData[];
    viewName: string;
  } {
    console.log(`\n=== ${this.getSectionName()} 섹션 렌더링 ===`);

    this.initialize();

    const cards = this.loadCards();

    this.validateCards(cards);

    const processedCards = this.processCards(cards);

    return this.finalizeRender(processedCards);
  }

  protected abstract getSectionName(): string;
  protected abstract getViewName(): string;
  protected abstract loadCards(): CardData[];

  protected initialize(): void {
    console.log(`  └─ ${this.getSectionName()} 초기화 중...`);
  }

  protected validateCards(cards: CardData[]): void {
    if (cards.length === 0) {
      console.warn(`  └─ 경고: ${this.getSectionName()}에 카드가 없습니다.`);
    }
    console.log(`  └─ ${cards.length}개의 카드 검증 완료`);
  }

  protected processCards(cards: CardData[]): CardData[] {
    console.log("  └─ 기본 카드 처리 로직 적용");
    return cards;
  }

  protected finalizeRender(cards: CardData[]): {
    title: string;
    cards: CardData[];
    viewName: string;
  } {
    console.log(`  └─ ${this.getSectionName()} 렌더링 완료`);
    return {
      title: this.getSectionName(),
      cards: cards,
      viewName: this.getViewName(),
    };
  }
}

class FoodSection extends CardSection {
  protected getSectionName(): string {
    return "추석의 대표 음식";
  }

  protected getViewName(): string {
    return "food";
  }

  protected loadCards(): CardData[] {
    return [
      {
        id: "food-1",
        title: "송편",
        description:
          "그 해 수확한 햅쌀로 빚어 조상께 감사하는 마음을 담은, 한가위의 상징적인 떡입니다.",
        imagePath: "./Gemini_Generated_Image_송편.png",
        imageAlt: "송편 이미지",
      },
      {
        id: "food-2",
        title: "모둠전",
        description:
          "동그랑땡, 산적, 동태전, 고추전 등 기름에 지진 전은 명절의 '풍요로움'을 상징합니다.",
        imagePath: "./Gemini_Generated_Image_전.png",
        imageAlt: "각종 전 이미지",
      },
      {
        id: "food-3",
        title: "갈비찜",
        description:
          "갈비찜은 조선시대 궁중 수라상에도 올랐던 고급 요리입니다. 이처럼 귀한 음식을 명절에 먹는다는 것은, 한 해의 수확에 감사하고 기쁨을 나누는 의미입니다.",
        imagePath: "./Gemini_Generated_Image_갈비찜.png",
        imageAlt: "갈비찜 이미지",
      },
    ];
  }

  protected processCards(cards: CardData[]): CardData[] {
    console.log("  └─ 음식 카드 특별 처리: 이미지 프리로딩");

    return cards;
  }
}

class CustomSection extends CardSection {
  protected getSectionName(): string {
    return "한가위 전통 풍습";
  }

  protected getViewName(): string {
    return "game";
  }

  protected loadCards(): CardData[] {
    return [
      {
        id: "custom-1",
        title: "강강술래",
        description:
          "밝은 보름달 아래, 여성들이 손을 잡고 원을 그리며 풍요와 안녕을 기원하던 놀이입니다.",
        imagePath: "./Gemini_Generated_Image_강강술래.png",
        imageAlt: "강강술래 이미지",
      },
      {
        id: "custom-2",
        title: "윷놀이",
        description:
          "가족, 친지들이 모여 편을 나누어 즐기는 대표적인 민속놀이로, 화합을 다집니다.",
        imagePath: "./Gemini_Generated_Image_윷놀이.png",
        imageAlt: "윷놀이 이미지",
      },
      {
        id: "custom-3",
        title: "차례",
        description:
          "햇곡식과 햇과일로 정성껏 상을 차려 조상께 감사를 전하는 중요한 의식입니다.",
        imagePath: "./Gemini_Generated_Image_차례상.png",
        imageAlt: "차례 이미지",
      },
    ];
  }

  protected initialize(): void {
    super.initialize();
    console.log("  └─ 전통 풍습 섹션 추가 초기화: 역사적 배경 로드");
  }
}

class StorySection extends CardSection {
  protected getSectionName(): string {
    return "추석 이야기";
  }

  protected getViewName(): string {
    return "story";
  }

  protected loadCards(): CardData[] {
    return [
      {
        id: "story-1",
        title: "달토끼 이야기",
        description:
          "추석의 상징인 보름달을 보며 누구나 한 번쯤 들어봤을 이야기입니다. 달에 토끼가 절구로 떡(혹은 불로장생의 약)을 찧고 있다는 상상이죠. 이는 불교 설화에서 유래된 이야기로, 자신을 희생해 노인을 도운 토끼가 그 공로로 옥황상제에 의해 달에 살게 되었다는 내용을 담고 있습니다.",
        imagePath: "./Gemini_Generated_Image_달토끼.png",
        imageAlt: "달토끼 이미지",
      },
      {
        id: "story-2",
        title: '"더도 말고 덜도 말고 한가위만 같아라"',
        description:
          "추석에 관한 가장 유명하고 상징적인 속담입니다. 덥지도 춥지도 않은 완벽한 날씨, 햇곡식과 햇과일이 가득한 풍요로움, 오랜만에 만난 가족 친지와의 즐거움까지, 일 년 중 가장 행복한 이 날처럼만 일 년 내내 살고 싶다는 소망이 담겨 있습니다.",
        imagePath: "./Gemini_Generated_Image_더말덜말.png",
        imageAlt: "속담 1 이미지",
      },
      {
        id: "story-3",
        title: '"오월 농부 팔월 신선" (五月農夫 八月神仙)',
        description:
          "이 속담은 추석이 '수확의 계절'임을 명확하게 보여줍니다. 음력 5월에는 씨 뿌리고 밭을 가꾸느라 농부가 가장 바쁘고 힘들지만, 음력 8월 한가위가 되면 풍성하게 거둔 곡식을 보며 신선처럼 여유롭고 즐겁게 지낸다는 의미입니다. 노력의 결실과 풍요로운 휴식을 잘 표현한 말입니다.",
        imagePath: "./Gemini_Generated_Image_오농팔신.png",
        imageAlt: "속담 2 이미지",
      },
    ];
  }

  protected processCards(cards: CardData[]): CardData[] {
    console.log("  └─ 이야기 카드 특별 처리: 텍스트 길이 최적화");
    return cards.map((card) => ({
      ...card,
      description:
        card.description.length > 200 ? card.description : card.description,
    }));
  }
}

class SectionFactory {
  private static sections: Map<string, CardSection>;

  static {
    this.sections = new Map<string, CardSection>();
    this.sections.set("food", new FoodSection());
    this.sections.set("game", new CustomSection());
    this.sections.set("story", new StorySection());
  }

  static getSection(viewName: string): CardSection | undefined {
    return this.sections.get(viewName);
  }

  static getAllSections(): CardSection[] {
    return Array.from(this.sections.values());
  }
}

const activeView = ref<"main" | "food" | "game" | "story">("main");
const greetingMessage = ref<string>(
  "보름달처럼 넉넉하고 풍요로운 마음으로,\n즐거운 추석 연휴 보내시길 기원합니다."
);

const foodData = ref<{ title: string; cards: CardData[] } | null>(null);
const customData = ref<{ title: string; cards: CardData[] } | null>(null);
const storyData = ref<{ title: string; cards: CardData[] } | null>(null);

onMounted(() => {
  console.log("=== Template Method 패턴 실행 ===");

  const foodSection = SectionFactory.getSection("food");
  const customSection = SectionFactory.getSection("game");
  const storySection = SectionFactory.getSection("story");

  if (foodSection) {
    const result = foodSection.render();
    foodData.value = { title: result.title, cards: result.cards };
  }

  if (customSection) {
    const result = customSection.render();
    customData.value = { title: result.title, cards: result.cards };
  }

  if (storySection) {
    const result = storySection.render();
    storyData.value = { title: result.title, cards: result.cards };
  }
});

const wishes: string[] = [
  "당신의 소원이 꼭 이루어지길 바랍니다.",
  "늘 건강하고 행복하세요!",
  "웃음꽃 가득한 명절 보내세요.",
  "보름달처럼 환한 일만 가득하길!",
];

const makeAWish = () => {
  const otherWishes = wishes.filter((w) => w !== greetingMessage.value);
  const pool = otherWishes.length > 0 ? otherWishes : wishes;
  const randomIndex = Math.floor(Math.random() * pool.length);
  greetingMessage.value = pool[randomIndex] as string;
};

const onImgError = (event: Event) => {
  const target = event.target as HTMLImageElement;
  target.src =
    "https://placehold.co/400x300/5a5a7d/f0e6d2?text=이미지+로드+실패";
};
</script>

<style scoped>
@import url("https://cdn.jsdelivr.net/gh/orioncactus/pretendard/dist/web/static/pretendard.css");

:global(body, html) {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow-x: hidden;
}

:global(#app) {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.chuseok-container {
  font-family: "Pretendard", sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  width: 100%;
  background: linear-gradient(to bottom, #0a0a23, #1a1a3d);
  color: #f0e6d2;
  padding: 2rem;
  box-sizing: border-box;
  text-align: center;
  overflow-x: hidden;
}

.header h1 {
  font-size: 3rem;
  font-weight: 700;
  margin: 0;
  color: #fff;
  text-shadow: 0 0 10px #fdf6e4;
}

.main-content,
.content-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 1000px;
  margin: 5rem 0;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.moon-wrapper {
  position: relative;
  animation: gentle-glow 4s ease-in-out infinite;
}

.moon {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background-color: #fdf6e4;
  box-shadow: 0 0 25px #fdf6e4, 0 0 50px #fdf6e4, 0 0 75px #fff;
  position: relative;
  overflow: hidden;
}

.moon-shadow {
  position: absolute;
  top: 25%;
  left: 15%;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background-color: rgba(0, 0, 0, 0.08);
  box-shadow: 15px 30px 0 rgba(0, 0, 0, 0.08),
    40px -10px 0 20px rgba(0, 0, 0, 0.06);
}

.greeting {
  font-size: 1.25rem;
  line-height: 1.8;
  margin-top: 5rem;
  max-width: 600px;
  white-space: pre-line;
  transition: opacity 0.3s ease-in-out;
}

.wish-button {
  font-family: "Pretendard", sans-serif;
  font-size: 1rem;
  font-weight: 600;
  color: #0a0a23;
  background-color: #f0e6d2;
  border: none;
  border-radius: 50px;
  padding: 0.8rem 1.8rem;
  margin-top: 2rem;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(240, 230, 210, 0.2);
}

.wish-button:hover {
  transform: translateY(-2px);
  background-color: #fff;
  box-shadow: 0 6px 20px rgba(240, 230, 210, 0.3);
}

.wish-button:active {
  transform: translateY(0);
}

.nav-buttons {
  display: flex;
  gap: 1rem;
  margin-top: 2.5rem;
}

.nav-button {
  font-family: "Pretendard", sans-serif;
  font-size: 0.9rem;
  font-weight: 500;
  color: #f0e6d2;
  background-color: transparent;
  border: 1px solid #f0e6d2;
  border-radius: 50px;
  padding: 0.6rem 1.2rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.nav-button:hover {
  background-color: rgba(240, 230, 210, 0.1);
  transform: translateY(-2px);
}

.content-section h2 {
  font-size: 2.2rem;
  font-weight: 700;
  color: #fff;
  margin-bottom: 2.5rem;
}

.card-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
  width: 100%;
}

.info-card {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 16px;
  padding: 1.5rem;
  text-align: left;
  backdrop-filter: blur(5px);
  -webkit-backdrop-filter: blur(5px);
  color: #f0e6d2;
  transition: all 0.3s ease;
}

.info-card:hover {
  transform: translateY(-5px);
  background: rgba(255, 255, 255, 0.08);
}

.card-img {
  width: 100%;
  height: auto;
  aspect-ratio: 16 / 9;
  object-fit: cover;
  border-radius: 10px;
  margin-bottom: 1rem;
  background-color: #5a5a7d;
}

.info-card h3 {
  font-size: 1.5rem;
  font-weight: 700;
  color: #fff;
  margin-top: 0;
  margin-bottom: 0.5rem;
}

.info-card p {
  font-size: 1rem;
  line-height: 1.6;
}

.back-button {
  font-family: "Pretendard", sans-serif;
  font-size: 1rem;
  font-weight: 600;
  color: #0a0a23;
  background-color: #f0e6d2;
  border: none;
  border-radius: 50px;
  padding: 0.8rem 1.8rem;
  margin-top: 2.5rem;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(240, 230, 210, 0.2);
}

.back-button:hover {
  transform: translateY(-2px);
  background-color: #fff;
  box-shadow: 0 6px 20px rgba(240, 230, 210, 0.3);
}

.footer {
  font-size: 1rem;
  color: #d0c0a0;
  margin-top: 2rem;
}

@keyframes gentle-glow {
  0% {
    transform: scale(1);
    opacity: 0.9;
  }
  50% {
    transform: scale(1.03);
    opacity: 1;
  }
  100% {
    transform: scale(1);
    opacity: 0.9;
  }
}

@media (max-width: 768px) {
  .header h1 {
    font-size: 2.2rem;
  }
  .moon {
    width: 150px;
    height: 150px;
  }
  .greeting {
    font-size: 1.1rem;
    margin-top: 2rem;
  }
  .wish-button {
    font-size: 0.9rem;
    padding: 0.7rem 1.5rem;
  }
  .nav-buttons {
    flex-direction: column;
    gap: 0.5rem;
  }
  .nav-button {
    font-size: 0.9rem;
    padding: 0.6rem 1.2rem;
  }

  .card-grid {
    grid-template-columns: 1fr !important;
  }
  .content-section h2 {
    font-size: 1.8rem;
  }
  .info-card h3 {
    font-size: 1.3rem;
  }
  .info-card p {
    font-size: 0.95rem;
  }
  .back-button {
    margin-top: 2rem;
  }
}
</style>
