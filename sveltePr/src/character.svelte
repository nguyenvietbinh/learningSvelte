<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher()
    let character, characterX = 100,
     characterY = 370, characterDirection = {
        left: false,
        right: false,
        jumpping: false,
     }, gravity = 0.5, velocity = 10, isPass = false
    function sendData() {
        dispatch('sendData', [characterX, characterY])
    }
    function move() {
        if (!isPass) {
            if (characterDirection.left) {
                if (characterX >= 4) {
                    characterX -= 4
                }
            }
            if (characterDirection.right) {
                if (characterX <= 1166) {
                    characterX += 4
                }
            }
            if (characterDirection.jumpping) {
                characterY -= velocity
                velocity -= gravity
                if (characterY >= 370) {
                    characterDirection.jumpping = false
                    characterY = 370
                    velocity = 10
                }
            }
        }
        sendData()
        isPassCheck()
        character.style.left = `${characterX}px`
        character.style.top = `${characterY}px`
        requestAnimationFrame(move)
    }
    function isPassCheck() {
        if ((characterX >= 1070 & characterX <= 1130) & (characterY >= 340)) {
            isPass = true
        }
    }
    import { onMount } from "svelte";
    onMount(() => {
        character = document.querySelector('.character')
        character.style.left = `${characterX}px`
        character.style.top = `${characterY}px`
        document.addEventListener('keydown', function(event) {
            if (event.keyCode === 65) {
                characterDirection.left = true
            }
            if (event.keyCode === 68) {
                characterDirection.right = true
            }
            if (event.keyCode === 32 || event.keyCode === 87) {
                if (!character.jumpping) {
                    characterDirection.jumpping = true
                }
            }
        })
        document.addEventListener('keyup', function(event) {
            if (event.keyCode === 65) {
                characterDirection.left = false
            }
            if (event.keyCode === 68) {
                characterDirection.right = false
            }
        })
        requestAnimationFrame(move)
        window.addEventListener('resize', function(event) {
            character.style.left = `${changeCoordinateSystem(characterX, characterY)[0]}px`
            character.style.top = `${changeCoordinateSystem(characterX, characterY)[1]}px`
        })
    })
</script>

<div class="character">
    <div class="rightEye"></div>
    <div class="leftEye"></div>
</div>

<style>
    .character {
        display: block;
        height: 30px;
        width: 30px;
        background-color: black;
        position: absolute;
    }
    .rightEye {
        display: block;
        height: 5px;
        width: 5px;
        background-color: rgb(255, 255, 255);
        border-radius: 50%;
        position: absolute;
        top: 8px;
        left: 6px;
    }
    .leftEye {
        display: block;
        height: 5px;
        width: 5px;
        background-color: aliceblue;
        border-radius: 50%;
        position: absolute;
        top: 8px;
        right: 6px;
    }
</style>