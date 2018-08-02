# Animating Between Components
When animating between components, sprites travel back and forth from two separate lists that know about each other. This means that when a sprite leaves one list and goes to another, the list that it goes to, or its endpoint, knows where the sprite started from. This also applies in the opposite direction. Therefore, sprites always have an initial and a final destination when animating between components. When two components know about each other, they can identify when a sprite is animating between them by checking the start or endpoint of that sprite.

## receivedSprites: 
When a sprite animates between lists it is a `removedSprite` on the list it started at, and an `insertedSprite` on the list ended at. If the sprite holds the same data value on both sides, it is considered a `receivedSprite` on the list that it is going to.
## sentSprites: 
Sent sprites are the reverse of `receivedSprites`. If the sprite stores the same data value on both sides while moving from one list to another, it will be considered a `sentSprite` on the list that it came from. 


<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-eh2d{background-color:#ffffff;border-color:inherit;vertical-align:top}
.tg .tg-47u2{font-weight:bold;background-color:#ffffff;border-color:inherit;vertical-align:top;text-align:left}
.tg .tg-7g6k{font-weight:bold;background-color:#ffffff;border-color:inherit;text-align:center;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-47u2">Name</th>
    <th class="tg-7g6k">Initial State</th>
    <th class="tg-47u2">Final State</th>
  </tr>
    <tr>
    <td class="tg-eh2d">Inserted</td>
    <td class="tg-eh2d">No</td>
    <td class="tg-eh2d">Yes</td>
  </tr>
  <tr>
    <td class="tg-eh2d">Kept</td>
    <td class="tg-eh2d">Yes</td>
    <td class="tg-eh2d">Yes</td>
  </tr>
  <tr>
    <td class="tg-eh2d">Removed</td>
    <td class="tg-eh2d">Yes</td>
    <td class="tg-eh2d">No</td>
  </tr>
  <tr>
    <td class="tg-eh2d">Received</td>
    <td class="tg-eh2d">Remote</td>
    <td class="tg-eh2d">Local</td>
  </tr>
  <tr>
    <td class="tg-eh2d">Sent</td>
    <td class="tg-eh2d">Local</td>
    <td class="tg-eh2d">Remote</td>
  </tr>
</table>




### Interruption Cases
In this demonstration, the "Delete with Undo" option shows what happens when an animation is interrupted. Choose this option, then delete an email to see what happens when the delete action is interrupted. An email that would have been a `removedSprite` becomes a `keptSprite`. 

### Beacons
You may have noticed that this demonstration uses `#animated-beacon`. For more on beacons, see [animated-beacon](../docs/api/components/animated-beacon). 

In this example, emails animate between Refresh (mail icon), Trash, and the Inbox. When an email is deleted, it is considered a `removedSprite`. If you refresh your inbox, the new email added is an `insertedSprite`. This scenario has two beacons, the refresh button and the trash button. 

{{#docs-demo as |demo|}}
    {{#demo.example name="one"}}
      {{#full-log-table as |fullLog|}}
        {{logged-between-components fullLog=fullLog}}      
      {{/full-log-table}}
    {{/demo.example}}

    {{demo.snippet 'between-components-snippet.hbs' label='between-components.hbs'}}
    {{demo.snippet 'between-components-snippet.js' label='between-components.js'}}
    {{demo.snippet 'sprites-snippet.css'}}
{{/docs-demo}}

### Animating Across Lists
In this example, the office is hosting a dinner party. Everyone received an email invitation with two options "going" and "not going". Many people are not sure if they want to go or not, and thankfully invitees can change their response as many times as they want before the party. If Dwight originally said he was going but then decides that he can no longer attend the dinner party, he will be removed from the "going" list and added to the "not going" list. This means that Dwight would be considered a `sentSprite` from the going list and a `receivedSprite` from the "not going" list. If Dwight then changes his mind and decides he will attend the dinner party, he will be removed from the "not going" list and added to the "going" list. In thic case, Dwight would now be considered a `sentSprite` from the "not going" list and a `receivedSprite` by the "going list". Both lists are aware of each other, therefore when he decides whether or not to go and and changes his mind, Dwight is always sent from one list and received by the other. 


{{#docs-demo as |demo|}}
    {{#demo.example name="two"}}
      {{#full-log-table as |fullLog|}}
        {{logged-two-lists fullLog=fullLog}}
      {{/full-log-table}}
    {{/demo.example}}

    {{demo.snippet 'between-two-lists-example-snippet.hbs' label='between-two-lists-example.hbs'}}
    {{demo.snippet 'between-two-lists-example-snippet.js' label='between-two-lists-example.js'}}
    {{demo.snippet 'two-lists-snippet.css'}}
{{/docs-demo}}


### Animating Across Routes
This is an example of animating sprites across different routes. When you select an icon from the list, both the selected image and the list of image animates while the route changes. 

{{#docs-demo as |demo|}}
    {{#demo.example name="hero"}}
      {{#animated-container class="debug"}}
        {{outlet}}
      {{/animated-container}}
    {{/demo.example}}

    {{demo.snippet 'hero-snippet.hbs' label='hero.hbs'}}
    {{demo.snippet 'detail-snippet.hbs' label='detail.hbs'}}
    {{demo.snippet 'detail-snippet.js' label='detail.js'}}
    {{demo.snippet 'index-snippet.hbs' label='index.hbs'}}
    {{demo.snippet 'index-snippet.js' label='index.js'}}
    {{demo.snippet 'hero-snippet.css'}}
{{/docs-demo}}