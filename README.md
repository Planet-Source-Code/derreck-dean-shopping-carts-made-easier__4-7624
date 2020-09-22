<div align="center">

## Shopping Carts made Easier


</div>

### Description

Explains tactics and some standards on programming e-Commerce applications, such as Shopping Carts.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Derreck Dean](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/derreck-dean.md)
**Level**          |Intermediate
**User Rating**    |4.4 (31 globes from 7 users)
**Compatibility**  |ASP \(Active Server Pages\)
**Category**       |[Coding Standards](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/coding-standards__4-33.md)
**World**          |[ASP / VbScript](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/asp-vbscript.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/derreck-dean-shopping-carts-made-easier__4-7624/archive/master.zip)





### Source Code

<font face="tahoma,verdana" size=2 style="font-size:8pt;"><B>Developing Smater Web Applications</b><BR><BR>
Here's some general guidelines when developing database-driven applications. I've learned these through experience.<BR><BR>
<b>(1) The "Master-Database Technique" is BAD</b><BR>
I developed a shopping cart system that held a lot of its information in a master database (how the site is configured, logins, etc.), and the products, categories, etc. were all kept in a site-specific database. Not only did I do that, I made the admin section global (in a single place) where all clients of the shopping cart systemn could log in and manage their website. In the beginning, this was a great idea; however, it now has its obvious flaws.<BR>
[a] What if a customer wants to move to a different hosting provider?<BR>
[b] What if the server that the master database resides on is different from the website's database server? If the master DB goes down, so does the website.
[c] The biggest problem was custom coding. It could not be done efficiently with the system I had in place. The code got so big and bulky that I am writing new code to replace it.<BR><BR>
<B>(2) Adding products to the consumer shopping cart: Direct DB access or Arrays?</b><BR>
When developing shopping carts, there are two general ways of handling how items are stored in the shopping cart. One is handling the products in an array that is held in a session variable. The other is directly putting the items into the database and using a unique identifier to handle the order. Let's look at both options here.<BR><BR>
<b>Array Shopping Cart</b><BR>
Pros:<BR>
 * EXTREMELY fast processing<BR>
 * Very easy to use<BR>
 * Uses little memory if done right (don't define a static array like "DIM Cart(7, 9999)", build dynamically using REDIM PRESERVE statements<BR>
Cons:<BR>
 * Unless you build it otherwise, you can't see what people have ordered before they up and leave without finishing
<BR><BR>
<b>Direct in DB Cart</b><BR>
Pros:<BR>
 * You can make wishlists or save orders for future visits/references without special programming (the records are already there)<BR>
 * By use of cookies you can defeat the session timeout of ASP sessions
Cons:<BR>
 * EXTREMELY SLOW (for a full featured cart, you can have a lot of database calls on one page)<BR>
 * CPU intensive compared to the array method<BR>
 * Since you have to make some kind of OrderID to refer the records to when they are added to the shopping cart, usually by INSERTing a blank order into the database, you'll find that you'll have a LOT of blank orders which will throw off your IDENTITY column (OrderID).<BR><BR>
The array shopping cart is better in this case. Personally, I would prefer to custom write code to do wishlists (not hard) or saving orders for future reference. That's what the Session_OnEnd sub is for!<BR><BR>
<b>(3) Product Customization Fields</b><BR>
This was a cool feature I added into my shopping carts. Some shopping carts out there only allowed you to customize certain features, such as Size or Color. I've seen other carts that allow you to do more than that BUT they were slow and bulky. Now, I introduce to you my method of writing product customization. For those of you in the dark here, product customization is letting the consumer choose certain options for his/her desired product. For example, a hat comes in three different colors, 2 different fabrics, and allows for engraved lettering on it. Some carts had a seperate table to keep track of these items in the cart. That's too much work. If you're good with working with Strings and string functions, it's not all that hard, and all you need is a text (nvarhcar) field, or a memo (ntext) field. Here's an example, using the previous defined data about the hat.<BR><BR>
Choose Color of Hat:=Black,White,Gray;Choose Fabric:=Suede,Cloth;Up to Three Letters for Engraving:=%text%;<BR><BR>
All I did was SPLIT() by the semicolon, and looped through and SPLIT() again by the equalsign to write out my fields. Here's some example code:<BR><BR>
DIM Atts<BR>
Atts = "Choose Color of Hat:=Black,White,Gray;Choose Fabric:=Suede,Cloth;Up to Three Letters for Engraving:=%text%;"<BR>
Heads = SPLIT(Atts, ";")<BR>
FOR A = LBOUND(Heads) TO UBOUND(Heads)<BR>
&nbsp;&nbsp;	F = SPLIT(Heads(A), "=")	' Splitting header and fields<BR>
&nbsp;&nbsp;	Response.Write F(0)		' Write header<BR>
&nbsp;&nbsp;	G = SPLIT(F(1), ",")		' Split fields<BR>
&nbsp;&nbsp;	IF UCASE(G(0)) = "%TEXT%" THEN<BR>
&nbsp;&nbsp;&nbsp;&nbsp;		' Textfield<BR>
&nbsp;&nbsp;&nbsp;&nbsp;		Response.Write "&lt;input type=""text"" name=""whatever"" value="""">"<BR>
&nbsp;&nbsp;	ELSE<BR>
&nbsp;&nbsp;&nbsp;&nbsp;		Response.Write "&lt;select name=""whatever"">"<BR>
&nbsp;&nbsp;&nbsp;&nbsp;		FOR B = 0 TO UBOUND(G)<BR>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	Response.Write "&lt;option>" & G(B) & "&lt;/option>"<BR>
&nbsp;&nbsp;&nbsp;&nbsp;		NEXT<BR>
&nbsp;&nbsp;&nbsp;&nbsp;		Response.Write "&lt;/select>"<BR>
&nbsp;&nbsp;	END IF<BR>
NEXT<BR><BR>
I was able to name the fields, read them when the form was submitted, stick them into an extra field in my array and later write them into the database. I even assigned pricing, where some options added/subtracted a price from the original price. There's lots of ways you can perform this.<BR><BR>
Vote if you like, and I'll post more soon!

