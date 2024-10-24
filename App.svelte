<script>
export let name, header_read, article_read, date_read, date_edit_read;
let i;
let headerName;
let textName;
let visible_list = false;
let visible_form = true;
let visible_edit = false;
let visible_article = false;
let names = [];
let edit_index;

//Создание новой статьи
function NewArticle() {
	let date = new Date().toLocaleString('en-GB', {
  hour12: false,
});
	let article = [headerName, textName, date]
	//let article = {name: headerName, age: textName, date: date}
	let string = JSON.stringify(article)
	if (localStorage.getItem("count") === null) {
    localStorage.setItem('count', 0)
	}
	i = localStorage.getItem('count')
	i = parseInt(i)
	localStorage.setItem('article_'+i, string)
	i += 1
	localStorage.setItem('count', i)
}

//Редактирование статьи
function EditArticle() {
    let article_info = JSON.parse(localStorage.getItem('article_'+edit_index))
 	let date = article_info[2]
 	let date_edit = new Date().toLocaleString('en-GB', {
  hour12: false,
});
	let article = [headerName, textName, date, date_edit]
	let string = JSON.stringify(article)
	localStorage.setItem('article_'+edit_index, string)
	visible_edit = false
	visible_form = true
}

//Переход в раздел списка статей
function ListVissible() {
    visible_list = false
    visible_form = true
	visible_edit = false
	visible_article = false
}

//Переход в раздел редактирования статьи
function EditVissible(ind) {
	visible_list = false
    visible_form = false
    visible_edit = true
    visible_article = false
    edit_index = ind

}

//Переход в раздел созданий статьи
function FormVissible() {
    visible_list = true
    visible_form = false
    visible_article = false
	visible_edit = false

    for (let j = 0; j < localStorage.length-1; j++) 
    {
    	let article_info = JSON.parse(localStorage.getItem('article_'+j))
 		//names[j] = article_info[0]

 		names[j] = {name: article_info[0], date:article_info[2], date_edit:article_info[3]}
	}
}

//Переход в раздел статьи
function ArtileVissible(ind) {
	visible_list = false
    visible_form = false
    visible_edit = false
    visible_article = true
    visible_article = true
    let article_info = JSON.parse(localStorage.getItem('article_'+ind))
   	header_read = article_info[0]
    article_read = article_info[1]
    date_read = article_info[2]
    date_edit_read = article_info[3]
}

</script>

<main>
	<h1>Hello, {name}!</h1>

<!--Раздел создания статьи-->
	{#if visible_form}
		<button name="form_button" on:click={FormVissible}>
  			Show the list
		</button><br>
	    <input id="name" type="text" placeholder ="header" bind:value={headerName}><br>
	 	<textarea id="text" placeholder ="text" cols=23 bind:value={textName}></textarea><br>
		<button type="submit" on:click={NewArticle}>Send</button><br>
	{/if}

<!--Раздел списка статей-->
	{#if visible_list}
		<button name="list_button" on:click={ListVissible}>
	    	Show the form
		</button><br>
	    <ul>
			{#each names as name, index}
			    <li>{index + 1}) Заголовок - {name.name}; Дата создания - {name.date}; 
			    {#if name.date_edit != null}
			    Дата редактирования - {name.date_edit}
				{/if }
			    <button name="edit_button" on:click={() => EditVissible(index)}>
	    			Edit
				</button>
			    <button name="edit_button" on:click={() => ArtileVissible(index)}>
	    			Read
				</button>
			    </li>
			{/each}
		</ul>
	{/if}

<!--Раздел редактирования статьи-->
	{#if visible_edit}
		<input id="name" type="text" placeholder ="header" bind:value={headerName}><br>
	 	<textarea id="text" placeholder ="text" cols=23 bind:value={textName}></textarea><br>
	 	<button type="submit" on:click={EditArticle}>Edit</button><br>
	{/if}

<!--Раздел чтения статьи-->
	{#if visible_article}
		<button name="list_button" on:click={FormVissible}>
	    	Show the List
		</button>
		<h2>{header_read}</h2>
		<h3>{article_read}</h3>
		Дата создания - {date_read}<br>

		{#if date_edit_read != null}
			Дата редактирования - {date_edit_read}
		{/if }

	{/if}

</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	ul {
	    text-align: center;
	}
	li {
 	   display: block;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>