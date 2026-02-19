# treasure
// ==============================
// Treasure Hunt Game
// ==============================

// ====== Helper Functions ======
function getValidNumber(message, min, max) {
  let num;
  do {
    num = Number(prompt(message));
  } while (isNaN(num) || num < min || num > max);
  return num;
}

function getRandomTreasure(mapSize, playerPos) {
  let treasure;
  do {
    treasure = Math.floor(Math.random() * mapSize);
  } while (treasure === playerPos);
  return treasure;
}

// ====== Game Object ======
const game = {
  mapSize: 0,
  playerPosition: 0,
  treasurePosition: 0,
  remainingMoves: 0,
  moveHistory: [],
  gameOver: false,

  move(direction) {
    if (this.gameOver) return;

    let oldPosition = this.playerPosition;

    if (direction === "left") {
      if (this.playerPosition > 0) {
        this.playerPosition--;
      } else {
        console.log("❌ لا يمكنك التحرك لليسار (خارج الخريطة)");
        return;
      }
    } else if (direction === "right") {
      if (this.playerPosition < this.mapSize - 1) {
        this.playerPosition++;
      } else {
        console.log("❌ لا يمكنك التحرك لليمين (خارج الخريطة)");
        return;
      }
    } else {
      console.log("❌ حركة غير صحيحة");
      return;
    }

    this.remainingMoves--;

    // Save move history
    this.moveHistory.push({
      direction,
      from: oldPosition,
      to: this.playerPosition,
      remainingMoves: this.remainingMoves
    });

    console.log(
      `➡️ تحركت ${direction} من ${oldPosition} إلى ${this.playerPosition}`
    );

    // Check win
    if (this.playerPosition === this.treasurePosition) {
      console.log("🎉 مبروك! لقد وجدت الكنز!");
      this.gameOver = true;
    }

    // Check lose
    if (this.remainingMoves === 0 && !this.gameOver) {
      console.log("💀 انتهت الحركات! خسرت اللعبة.");
      this.gameOver = true;
    }
  }
};

// ====== Game Setup ======
game.mapSize = getValidNumber("أدخل حجم الخريطة (مثلاً 7):", 2, 50);
game.remainingMoves = getValidNumber("أدخل عدد الحركات:", 1, 100);
game.playerPosition = getValidNumber(
  `أدخل موقع البداية (0 إلى ${game.mapSize - 1}):`,
  0,
  game.mapSize - 1
);

game.treasurePosition = getRandomTreasure(
  game.mapSize,
  game.playerPosition
);

// ====== Moves List ======
const moves = ["right", "right", "left", "right", "right", "left"];

// ====== Game Loop ======
for (let move of moves) {
  if (game.gameOver) break;
  game.move(move);
}

// ====== Final Output ======
console.log("===== النتيجة النهائية =====");
console.log(
  game.playerPosition === game.treasurePosition
    ? "✅ فزت باللعبة"
    : "❌ خسرت اللعبة"
);

console.log("📍 موقع الكنز:", game.treasurePosition);
console.log("📜 تاريخ الحركات:");
console.table(game.moveHistory);
console.log("🏁 نهاية اللعبة");
