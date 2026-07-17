first commit

So here is the plan:
this is going to be a relitivly easy and simple frontend, and the more impressive (and cool) part about this will be the backend
ultimatly I want to have this database in kube cluster that I have been working on. I would really like to be working on that rightaway, but as a am currently no where close to those computers, I can not yets really get it working the way I want it to be, and I cant really ask my mom to fill in for me.

The general outline of this project and timeline is as follows:
    build very basic frontend
    outline the database and the feilds I want
    have a local database 

SQL Database with the following starting fields:

``` markdown
protein_breakdown(__protein_breakdown_id__, histidine, isoleucine, leucine, lysine, methionine, phenylalanine, threonine, tryptophan, valine) // in terms of g per 100g, yeah this will mostly be in mgs, but im commiting to g/100g
Ingredient(__ingredient_id__, name, calories, protein, fat, fiber, **fk(protein_breakdown)**) // everying g/100g
needed_ingredients(__recipe_id__, __recipe_id__, mass, **count**) **key=__recipe_id__,__recipe_id__** // mass in grams, count optional
recipe(__recipe_id__, name, ingredients, recipe, owner) **fk(owner from user.user_id, ingredients from needed_ingredients)** // only owner can edit. ingredients are select * from needed_ingredients where recipe_id = recipe_id. recipe might be a txt file? might be a pointer to a markdown file? might json obj? who knows?
user(__user_id__, __username__, password) **key=__user_id__** // for now it is just me, also I have not tought about how to secure this yet, again, I am the only user for a long long time, ill think about it later
user_ingredient(__user_ingredient_id__, user_id, bought, expires, mass, **count**) **fk(user_id from user)** // count optional, bought and expires are DATE
```

below is a stream of consiousness, some good stuff, to in the brainstorm zone to edit
    Storage() // I have two fridges and freezers. I dont know if this one would be one that sticks. It could easily fall out of sync and its something that I would not really care about. I think I will narrow it down to just fridge, freezer, pantry. soemthing that i have to consider is the need for spices. If for refilling, that is something that I am typically on top of, but If i dont have something, I dont have it. there are also a few spice blends that I addore, but I have to make it myself because my ratio is just that good. 
    // storage is essentailly going to be just a list of ingredients, with speial quantites expiry dates. I think it is still vital to tell if it is frozen. that addes a few hours if the user does not take it out beforehand
    My_Ingredients()
    Garden()
    Recipe(__recipe_id__, ingredients, ) // ingredients 
    // this will have to be a lot of guess work. I would like to have all of this be super acurate, but i know myself. I cant follow a recipe if it means not ruining several meals. This will have a lot of informtation so that I will be able to gleen general information
    // I think this feild kinda breaks the idea of sql, but i still want things to be relational. I am imagining something like markdown / obsidian for the information and details.

    Ingredient(__ngredient_id__, name, cal, protein, fat, fiber, **fk(protein_breakdown)**) // this is going to be immutable (for non admin) and only visible when someone is trying to make a receipe or log there own food. includes guesstimates of the macro breakdown - not in a ton of detail, but enough to get the basics. this, like a proper country's nutrition facts, will list the contents per 100 grams. Now how much of a nerd do I want to be about this? Massive. Proteins require all 9 essential amino acids for protein synthosis. 
    Protein_Breakdown(__protein_breakdown_id__, histidine, isoleucine, leucine, lysine, methionine, phenylalanine, threonine, tryptophan, valine) // this is important information when it comes to replacing protein sources from meat (which is almost always a complete protein -collagen, way too many people suppliment this thinking they are benifiting protein muscle synthosis) I wont pretend that I understand what each of them do, but i know you need an equal amount of each. When people say collagen is useless because it is not a complete protein, they are wrong. It is low in tryptophan so if that is all you ate for days, yes you would be getting almost no protein. but you can suppliment with something like Spirulina (tho this is not cheap, and will only get more expensive as this is the gold standard for natural blue die, and recently rfk made it so that we can not have petrolium based dies. Good thought, but currently we dont even have enough spirulina to cover blue gatorade. plus its exclusivly made in China and the terifs wont help either) or pumpkin seeds that have a higher amount of tryptophan to make up for this imbalence. keeping all of these things in mind will make the transition from meat for protein to vegan much easier (shout out all the rise in lone star cases!)
    
    // expire date if it is included, otherwise it will guess off the the purchase date and the type of food that it is (hard, genuinly might have to have to hit an LLM for this, makes things a lot easier than manually going about this. I want to restric AI in this project as much as possible, if I do go this route, I will make an api for a local LLM). 
    //Weight will be in grams. None of this pounding ounches sillies


Based off of what a user has avalible to them, it will list all of the receipes that they have saved that would be useful for them. there are a ton of apps that I see advertised they say send them a link of an instagram and they will get the recipe. my bet is that they just throw it into some ai that scrapes it or guesses. this is not one of those projects
Users should be able to search for recipies 
Keep in mind how many people you are trying to cook for / how many total calories you are aiming for. this might be important if a recipe is made with 500g of beef but serves 5 (serves never works for me personally, I would much rather have a guess of the cals in a meal and then work with it from there)
* color code if you do not have all of the ingredients. this should also be in classes, cause if you dont have avacado oil but have olive, this should not stop you at all. but if it calls for beef and you have . something like (i) you might be able to make this work
* indicate if some of the ingredients are in the freezer. this might make someone think something entirly different for dinner if they do not have the time to let soemthing thaw
display the complete proteins that someone has consumed in a day / week

Orginizing my pipe dreams to make the logging of this information much faster but they are just way out of scope of this project right now. These are pain points for users that I already know essist just by thinking about how the average user would want to put away there grosseries
    [ ] CV to take a picture of your grossery receipt and log as much information as possible from there, would not take the pain out of the original logging, but it would make it so much easier to make sure that it is kept up to date
    [ ] Scanner for the barcodes (amazon did this so it cant be that hard) to log faster. Keep the specific code 
    Scale that connects and updates the weight of my food. much better than an eyeballing or hefting. much better than logging the numbers myself, but this is something that would either require a crazy expensive scale or some fun mcguivering. One way I would thing to solve it is to have the scale set to grams and have and esp32 read the output to the 7 seg displays. I dont need to know anything more that hyjacking these values, and then disecting them down. then have it interface with
    [ ] Cool interface for the making of receipes. I am thinking something that will be able to be markdowned. Prefece something is an ingredient, and then it will fill in. this will avoid the need for manually saying what ingredients are needed at the start. this way the backend will know all about it even though the user does not explicetly do anything for it
    [ ] Have a few options to be able to swap ingredients (this would also be something that filling out would not be very good for a human to do, this would be a good application of an LLM )
    [ ] Historical data. see how many calories a day, protein, fiber average. And it would be cool to see how much food you have waisted over the week (this is not really a problem for me and the composter, maybe seperate out the meat)
    [ ] make a whole big meal and be able to say I ate about 40% and then tmr I ate 30%