<script lang="ts">
	import { onMount } from "svelte";

    let dragged = $state<HTMLElement>()
    
    const dragHandlers = {
        drag: (e: Event) => {},
        dragStart: () => { console.log("(1) 드래그 시작")},
        dragEnter: (e: DragEvent) => {
            //핸들러가 붙어있는 대상이 e.currentTarget이 된다.
            e.preventDefault()
            const target = e.currentTarget as HTMLInputElement
            target.classList.add('dragover')

        },
        dragLeave: (e: DragEvent) => {
            //드래그 가능한 요소가 대상 밖으로 나가면 강조를 제거한다.
            const target = e.currentTarget as HTMLInputElement
            target.classList.remove('dragover')
        },
        dragOver: (e: DragEvent) => {
            e.preventDefault()
        },
        drop: (e: Event) => {
            const target = e.target as HTMLInputElement
            if (target.classList.contains('dropzone')){
                target.classList.remove('dragover')
                dragged?.parentElement?.removeChild(dragged)
                if (dragged)target.appendChild(dragged)
            }
        }
    }

    onMount(()=>{
        document.addEventListener('drop', dragHandlers.drop )
        return () => {
            document.removeEventListener('drop', dragHandlers.drop)
        }
    })

</script>



<div class="dropzone" 
    ondragenter={dragHandlers.dragEnter} 
    ondragleave={dragHandlers.dragLeave}
    ondragover={dragHandlers.dragOver}
>
    <div bind:this={dragged} id="draggable" draggable="true" 
        ondragstart={dragHandlers.dragStart}
    >드래그 가능</div>
</div>
<div class="dropzone"
    ondragenter={dragHandlers.dragEnter} 
    ondragleave={dragHandlers.dragLeave}
    ondragover={dragHandlers.dragOver}
></div>

<style>
    #draggable {
        text-align: center;
        background: white;
    }

    .dropzone {
        width: 200px;
        height: 20px;
        background: blueviolet;
        margin: 10px;
        padding: 10px;


    }

    .dropzone.dragover {
        background-color: purple;
    }

    .dragging {
        opacity: 0.8;
    }
</style>