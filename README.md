var monkey, scene, scene2, banana2, obstacle2;
var monkey2, invisibleGround;
var obstacleGroup, foodGroup, score, monkeyCollided;
var PLAY = 1,
  END = 0,
  gameState = PLAY,
  gameOver;
var gameOver2, restart2;

function preload() {
  scene2 = loadImage("jungle.jpg");
  obstacle2 = loadImage("stone.png");
  banana2 = loadImage("banana.png");
  monkey2 = loadAnimation("Monkey_01.png", "Monkey_02.png", "Monkey_03.png", "Monkey_04.png", "Monkey_05.png", "Monkey_06.png", "Monkey_07.png", "Monkey_08.png", "Monkey_09.png", "Monkey_10.png");
  gameOver2 = loadImage("gameOver.png");
}

function setup() {
  createCanvas(400, 400);

  scene = createSprite(200, 200, 10, 70);
  scene.addImage("paddle", scene2);
  scene.scale = 0.8;
  scene.width = scene.width / 2;

  monkey = createSprite(50, 340, 10, 10);
  monkey.addAnimation("monkey", monkey2);
  monkey.scale = 0.1;

  invisibleGround = createSprite(200, 370, 400, 1);
  invisibleGround.visible = false;

  gameOver = createSprite(200, 200);
  gameOver.addImage(gameOver2);

  gameOver.scale = 1;

  gameOver.visible = false;

  obstacleGroup = new Group();
  foodGroup = new Group();

  score = 0;
}

function draw() {
  background("white");

  if (gameState === PLAY) {

    scene.velocityX = -2;
    monkey.collide(invisibleGround);

    if (scene.x < 0) {
      scene.x = scene.width / 2;
    }

    if (keyDown("space") && monkey.y >= 200) {
      monkey.velocityY = -10;
    }

    monkey.velocityY = monkey.velocityY + 0.3;

    if (foodGroup.isTouching(monkey)) {
      foodGroup.destroyEach();
      score = score + 2;
    }

    switch (score) {
      case 10:
        monkey.scale = 0.12;
        break;
      case 20:
        monkey.scale = 0.14;
        break;
      case 30:
        monkey.scale = 0.16;
        break;
      case 40:
        monkey.scale = 0.18;
        break;

      default:
        break;
    }

    if (obstacleGroup.isTouching(monkey)) {
      gameState = END;
    }

    obstacles();
    bananas();

  } else if (gameState === END) {
    monkey.visible = false;
    scene.velocityX = 0;
    obstacleGroup.setVelocityXEach(0);
    foodGroup.setVelocityXEach(0);
    obstacleGroup.destroyEach();
    foodGroup.destroyEach();
    obstacleGroup.setLifetimeEach(-1);
    foodGroup.setLifetimeEach(-1);
    gameOver.visible = true;
  }

  drawSprites();

  fill("white");
  stroke("white");
  strokeWeight(2);
  textSize(20)
  text("Score: " + score, 180, 50);
}

function obstacles() {
  if (World.frameCount % 300 === 0) {
    var obstacle = createSprite(400, 350, 10, 70);
    obstacle.addImage("stone", obstacle2);
    obstacle.velocityX = -2;
    obstacle.scale = 0.09;
    obstacle.depth = scene.depth;
    obstacle.depth = obstacle.depth + 1;

    obstacleGroup.add(obstacle);
  }
}

function bananas() {
  if (World.frameCount % 200 === 0) {
    var banana = createSprite(400, 200, 10, 70);
    banana.addImage("banana", banana2);
    banana.velocityX = -2;
    banana.scale = 0.05;
    banana.y = random(120, 200);
    banana.depth = scene.depth;
    banana.depth = banana.depth + 1;

    foodGroup.add(banana);
  }
}
