# Third Party API Tutorial: Cocktails DB
## By: Cordelia Martinez Jaime

### What my API does
The API I am using is from a cocktail database found here: <a href="https://www.thecocktaildb.com/api.php">Documentation</a>. What my API does is display a list of all the cocktails within this database that have a certain type of alcohol as an ingredient. For example, if your ingredient, the variable in this case, is Vodka, then you will get a full list of cocktails that include Vodka. For Vodka, some of these include a Bloody Mary, Bellini Martini, and many more. 

---

### Code Examples
#### in web.php:
```
 Route::get('/cocktails/{ingredient}', function($ingredient) {
    $response = Cache::remember("cocktails-$ingredient", 60, function() use ($ingredient) {
        return Http::get("https://www.thecocktaildb.com/api/json/v1/1/filter.php?i=$ingredient")->object();
    });

    return view('api.cocktails', [
        'response' => $response,
        'ingredient' => $ingredient,
    ]);

});
```
#### in the cocktails.blade.php view:
```
<h1>Drinks with {{$ingredient}}</h1>
<ol>
    @foreach($response->drinks as $drink)
        <li>{{$drink->strDrink}}</li>
    @endforeach
</ol>
```
---

### API keys or tokens
There is a free testing version of this API that uses the key ‘1’ for educational purposes. However if you want a production API key then you have to sign up for a membership with the following link: https://www.patreon.com/thedatadb.
<br>
<img width="200" alt="cocktailsmember" src="https://user-images.githubusercontent.com/97271216/162346104-e09596f9-1620-4758-a46c-f46c0cdabfb8.png">
<br>
In this case, if I were to get an API key for production then I would include it in my .env file as a COCKTAIL_API_KEY and change my Http get within my response to:
```
return Http::withToken(env('COCKTAIL_API_KEY'))
        ->get('https://www.thecocktaildb.com/api/json/v1/1/filter.php?i=$ingredient')
        ->json();
```
---

### JSON response
<img width="300" alt="Screen Shot 2022-04-07 at 6 21 34 PM" src="https://user-images.githubusercontent.com/97271216/162346576-7882ecda-ead6-4fd4-a72e-a3a5b7bd212b.png">
Each drinks result includes:

1. Cocktail Name (strDrink)
2. Cocktail Thumbnail (strDrinkThumb)
3. Cocktail ID (idDrink)

which is why we can use the foreach drinks.

---

### When I ran my code for Vodka:
<img width="200" alt="Screen Shot 2022-04-07 at 6 44 51 PM" src="https://user-images.githubusercontent.com/97271216/162346859-c929a2af-7ead-4bdf-8785-a2801cd25d1c.png">



