<script lang="ts">
    import { onMount } from "svelte";
    import * as PIXI from "pixi.js";
    import "@pixi/graphics-extras";

    // Initialise variables to used in PIXI
    let app: PIXI.Application<HTMLCanvasElement>; // Pixi application
    let snake: PIXI.Graphics; // Piece of the snake body (Square)
    let food: PIXI.Graphics; // The food (Star)
    let foodX: number; // The x coordinate of the food
    let foodY: number; // The y coordinate of the food
    export let title: string = "Snake"; // The title that shows in the menu
    const snakeSize: number = 25; // Size of the snake body
    const foodSize: number = 12.5; // Size of the food
    const snakeBody: { x: number; y: number }[] = []; // Array that store the coordinates of all snake's body
    var Gameover: boolean = true; // Game status
    var direction: string = "down"; // Direction that the snake moves
    let elapsed: number = 0; // Timer that keeps the game going
    let pressed: boolean = false; // Condition that only allow user to press once per frame

    onMount(() => {
        // Create a PIXI Application
        app = new PIXI.Application<HTMLCanvasElement>({
            width: 509,
            height: 384,
            background: "#1099bb",
        });
        const element = document.getElementById("pixi-container");
        if (element) {
            // Append the PIXI Application's view (canvas)
            element.appendChild(app.view);
        } else {
            console.log("Fail to Append the PIXI application on the screen");
        }
        // Initialise PIXI component
        snake = new PIXI.Graphics();
        food = new PIXI.Graphics();

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
        app.ticker.maxFPS = 60;
        app.ticker.add((delta) => {
            elapsed += Math.round(delta);
            // console.log(delta > 2 ? delta : "");
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
    function storeCoordinate(xVal: number, yVal: number, array: object[]): void {
        array.push({ x: xVal, y: yVal });
    }

    // Draw food into the screen at a random place within the game container
    function drawFood(): void {
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
        food.drawStar?.(14.5 + foodX * 25, 15 + foodY * 25, 4, foodSize, 6); //Used optional chaining here as drawStar function might be undefine
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

    // Reset the game components
    function resetRound() {
        direction = "down";
        food.clear();
        snakeBody.length = 0;
        elapsed = 0;
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
    function onKeyDown(event: KeyboardEvent) {
        if (!Gameover && !pressed) {
            // W Key is 87 and Up arrow is 38
            if (event.key === "w" || event.key === "ArrowUp") {
                if (direction !== "down") {
                    direction = "up";
                }
            }
            // S Key is 83 and Down arrow is 40
            if (event.key === "s" || event.key === "ArrowDown") {
                if (direction !== "up") {
                    direction = "down";
                }
            }
            // A Key is 65 and Left arrow is 37
            if (event.key === "a" || event.key === "ArrowLeft") {
                if (direction !== "right") {
                    direction = "left";
                }
            }
            // D Key is 68 and Right arrow is 39
            if (event.key === "d" || event.key === "ArrowRight") {
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
        if (menu) {
            menu.style.display = "none";
        }
    }

    // Show the game over screen
    function showResult() {
        let menu = document.getElementById("menu");
        if (menu) {
            menu.style.background = "rgba(166, 166, 166, 0.6)";
            menu.style.display = "flex";
        }
        title = "GAME OVER!";
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
    <div id="buttonContainer">
        <button
            id="startbutton"
            on:click={newRound}>Play</button
        >
    </div>
</div>

<style>
    #menu {
        position: absolute;
        width: 509px;
        margin-bottom: 4px;
        height: 384px;
        display: flex;
        align-items: center;
        text-align: center;
        flex-direction: column;
    }

    #buttonContainer {
        position: absolute;
        bottom: 40px;
    }
    #startbutton {
        display: inline-block;
        background: none;
        font-family: "Pacifico", sans-serif;
        font-size: 32px;
        line-height: 0.8;
        padding: 10px 6px 20px 6px;
        border: 0;
        border-radius: 12px;
        cursor: pointer;
    }
    #startbutton:hover {
        background-color: black;
        color: #1099bb;
    }

    h1 {
        font-size: 4em;
        font-family: "Pacifico", sans-serif;
        white-space: normal;
        width: 80%;
        line-height: 80px;
        color: black;
    }
</style>
