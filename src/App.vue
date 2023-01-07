<script setup>
	import { ref } from "vue"

	let griglia = ref([
		['', '', ''],
		['', '', ''],
		['', '', '']	
	])
	let giocatoreCorrente = 'X'
	let vincitore = null
	let pareggio = null
	let inCorso = true
	let bottoneReset = ref(false)


	function handleCellClick(indiceRiga, indiceCella) {
		if (griglia.value[indiceRiga][indiceCella] || vincitore) {
			return
		}else{
			griglia.value[indiceRiga][indiceCella] = giocatoreCorrente
		}

		if (checkVittoria()) {
			vincitore = giocatoreCorrente
		} else if (checkPareggio()) {
			pareggio = true
		} else {
			giocatoreCorrente = giocatoreCorrente === 'X' ? 'O' : 'X'
		}
    }

	const serieVincenti = [
		[0, 1, 2],//prima riga
		[3, 4, 5],//seconda riga
		[6, 7, 8],//terza riga
		[0, 3, 6],//prima colonna
		[1, 4, 7],//seconda colonna
		[2, 5, 8],//terza colonna
		[0, 4, 8],//prima diagonale
		[2, 4, 6],//seconda diagonale
	]

	function checkVittoria(){
		for (let i = 0; i < serieVincenti.length; i ++){
			const [cella1, cella2, cella3] = serieVincenti[i]

			let grigliaFlat = griglia.value.flat()

			if (grigliaFlat[cella1] && grigliaFlat[cella1] === grigliaFlat[cella2] && grigliaFlat[cella1] === grigliaFlat[cella3]){
				inCorso = false
				bottoneReset = true
				return giocatoreCorrente;
			}
		}
		return null
	}

	function checkPareggio(){
		let grigliaFlat = griglia.value.flat()

		if (!grigliaFlat.includes('')){
			inCorso = false
			bottoneReset = true
			return true
		}else{
			return 
		}
	}

	function resettaPartita(){
		griglia.value = [
			['', '', ''],
			['', '', ''],
			['', '', '']
		]

		giocatoreCorrente = 'X'
		vincitore = null
		pareggio = null
		inCorso = true
		bottoneReset = false
	}

</script>


<template>
	<div class="container">
		<div class="titolo">
			<h1>Tic Tac Toe</h1>
			<div v-if="inCorso" class="descrizione">
				È il turno del giocatore: {{ giocatoreCorrente }}
			</div>
			<div class="risultato" v-if="vincitore">
				Il giocatore {{ vincitore }} ha vinto la partita
			</div>
			<div class="risultato" v-if="pareggio">
				Pareggio!
			</div>
		</div>

		<table class="tabella">
			<tr v-for="(riga, indiceRiga) in griglia" :key="indiceRiga">

				<td v-for="(cella, indiceCella) in riga" :key="indiceCella" @click="handleCellClick(indiceRiga, indiceCella)"> {{ cella }} </td>

			</tr>
		</table>
		<button @click="resettaPartita" :class="{bottoneReset: bottoneReset}">Ricomincia la partita</button>

		
	</div>
</template>


<style scoped>
	* {
		padding: 0;
		margin: 0;
		box-sizing: border-box;
	}

	.container {
		width: 100vw;	
		height: 100vh;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	.titolo{
		font-size: 32px;
		position: absolute;
		top: 2%;
		text-align: center;
	}
	.titolo .descrizione{
		font-size: 26px;
		margin-top: 5px;
	}
	.titolo .risultato{
		font-size: 33px;
		text-decoration: underline;
	}

	.tabella {
		margin-top: 10vw;
		border-collapse: collapse;
		table-layout: fixed;
		font-size: 125px;
		font-size: 7vw;
	}
	.tabella td {
		width: 12vw;
		height: 12vw;
		border: 5px solid rgb(40, 38, 38);
		padding: 0;
		text-align: center;
	}

	button{
		margin-top: 3vw;
		padding: 1vw 3vw;
		
		border-radius: 10px;
		border: 0;
		cursor: pointer;
		font-size: 20px;
		font-weight: 500;
		background-color: rgb(85, 80, 80);
		transition: transform .2s; /* Animation */

	}

	.bottoneReset {
		background-color: rgb(125, 117, 117);
	}
	button:hover {
		transform: scale(1.05);
	}
</style>