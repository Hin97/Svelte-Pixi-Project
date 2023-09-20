<script>
    // @ts-nocheck

    import { onMount } from "svelte";
    import * as PIXI from "pixi.js";
    import { Graphics } from "@pixi/graphics";
    import "@pixi/graphics-extras";

    // Initialise variables to used in PIXI
    let app, snake, food, foodX, foodY;
    export let title = "Snake";
    const snakeSize = 25;
    const foodSize = 12.5;
    const snakeBody = [];
    var Gameover = true;
    var direction = "down";
    let elapsed = 0.0;
    let pressed = false;

    onMount(() => {
        // Create a PIXI Application
        app = new PIXI.Application({
            width: 509,
            height: 384,
            background: "#1099bb",
        });
        // Append the PIXI Application's view (canvas)
        document.getElementById("pixi-container").appendChild(app.view);

        // Initialise PIXI component
        snake = new PIXI.Graphics();
        food = new Graphics();

        // Set up the game border
        const border = new PIXI.Graphics();
        border.lineStyle(3, "#000000", 1);
        border.beginFill("#1099bb");
        border.drawRect(0, 0, 505, 380);
        border.endFill();

        // Set up the container for the game
        const gameContainer = new PIXI.Container();
        gameContainer.position = { x: 2, y: 2 };
        gameContainer.width = 505;
        gameContainer.height = 380;

        // Render everything into the stage and container
        gameContainer.addChild(border);
        gameContainer.addChild(snake);
        gameContainer.addChild(food);
        app.stage.addChild(gameContainer);

        // Run the game at the back
        // if (!Gameover) {
        app.ticker.add((delta) => {
            elapsed += Math.round(delta);
            if (elapsed % 10 == 0 && Gameover == false) {
                updateSnake();
                checkPosition();
                drawSnake();
                pressed = false;
            }
        });
        // }
    });

    // Store snake body coordinate as an object
    // Snake body will be record as block number
    // X have 0-19 block and Y have 0-14 block
    function storeCoordinate(xVal, yVal, array) {
        array.push({ x: xVal, y: yVal });
    }

    // Draw food into the screen at a random place within the game container
    function drawFood() {
        food.clear();
        foodX = Math.floor(Math.random() * 20);
        foodY = Math.floor(Math.random() * 15);
        for (let i = 0; i < snakeBody.length; i++) {
            if (snakeBody[i].x == foodX && snakeBody[i].y == foodY) {
                drawFood();
            }
        }
        food.lineStyle(0);
        food.beginFill("#00FF00", 1);
        food.drawStar(14.5 + foodX * 25, 15 + foodY * 25, 4, foodSize);
        food.endFill();
    }

    // Draw the snake on the stage
    function drawSnake() {
        if (snakeBody.length && !Gameover) {
            snake.clear();
            for (let i = 0; i < snakeBody.length; i++) {
                snake.beginFill("#2a2b2a");
                snake.drawRoundedRect(2 + snakeBody[i].x * snakeSize, 3 + snakeBody[i].y * snakeSize, snakeSize, snakeSize, 8);
                snake.endFill();
            }
        }
    }

    // Update the snake position based on the direction that it moves
    function updateSnake() {
        let prevX = snakeBody[0].x;
        let prevY = snakeBody[0].y;
        for (let i = 1; i < snakeBody.length; i++) {
            let tempX = snakeBody[i].x;
            let tempY = snakeBody[i].y;
            snakeBody[i].x = prevX;
            snakeBody[i].y = prevY;
            prevX = tempX;
            prevY = tempY;
        }
        switch (direction) {
            case "down":
                snakeBody[0].y += 1;
                break;
            case "left":
                snakeBody[0].x -= 1;
                break;
            case "right":
                snakeBody[0].x += 1;
                break;
            case "up":
                snakeBody[0].y -= 1;
                break;
        }
    }

    function resetRound() {
        direction = "down";
        food.clear();
        snakeBody.length = 0;
    }

    // Check whether the snake hits the wall or bites itself or gets the food
    function checkPosition() {
        if (snakeBody[0].x < 0 || snakeBody[0].x > 19 || snakeBody[0].y < 0 || snakeBody[0].y > 14) {
            Gameover = true;
            showResult();
        }
        for (let i = 1; i < snakeBody.length; i++) {
            if (snakeBody[0].x == snakeBody[i].x && snakeBody[0].y == snakeBody[i].y) {
                Gameover = true;
                showResult();
            }
        }
        if (Gameover == false) {
            if (snakeBody[0].x == foodX && snakeBody[0].y == foodY) {
                storeCoordinate(foodX, foodY, snakeBody);
                drawFood();
            }
        }
    }

    // Keyborad listener function to control the direction of the snake move
    function onKeyDown(key) {
        if (!Gameover && !pressed) {
            // W Key is 87 and Up arrow is 38
            if (key.keyCode === 87 || key.keyCode === 38) {
                if (direction !== "down") {
                    direction = "up";
                }
            }
            // S Key is 83 and Down arrow is 40
            if (key.keyCode === 83 || key.keyCode === 40) {
                if (direction !== "up") {
                    direction = "down";
                }
            }
            // A Key is 65 and Left arrow is 37
            if (key.keyCode === 65 || key.keyCode === 37) {
                if (direction !== "right") {
                    direction = "left";
                }
            }
            // D Key is 68 and Right arrow is 39
            if (key.keyCode === 68 || key.keyCode === 39) {
                if (direction !== "left") {
                    direction = "right";
                }
            }
        }
        pressed = true;
    }

    // Hide the menu when the game starts
    function hideMenu() {
        let menu = document.getElementById("menu");
        menu.style.display = "none";
    }

    // Show the game over screen
    function showResult() {
        let menu = document.getElementById("menu");
        menu.style.background = "rgba(166, 166, 166, 0.7)";
        menu.style.display = "block";
        title = "GAMEOVER!";
    }

    // Start a new round of the game
    function newRound() {
        resetRound();
        Gameover = false;
        hideMenu();
        storeCoordinate(10, 4, snakeBody);
        storeCoordinate(10, 3, snakeBody);
        storeCoordinate(10, 2, snakeBody);
        storeCoordinate(10, 1, snakeBody);
        drawFood();
    }
</script>

<svelte:window on:keydown|preventDefault={onKeyDown} />
<div id="pixi-container" />

<div id="menu">
    <h1>{title}</h1>
    <button on:click={newRound}>Play</button>
</div>

<style>
    #menu {
        position: absolute;
        text-align: center;
        width: 509px;
        margin-bottom: 1px;
        height: 384px;
    }

    h1 {
        padding: 10px;
        margin-top: 110px;
        font-size: 2em;
    }
</style>
