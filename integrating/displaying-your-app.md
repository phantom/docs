# Displaying Your App

![Connection and transaction approval dialog for mango.markets](../.gitbook/assets/image.png)

When you are [Establishing a Connection](establishing-a-connection.md), or [Sending a Transaction](sending-a-transaction.md) to a user with Phantom, they are presented with the dialogs above. Phantom will inspect the HTML of the application's page itself in order to display the title and icon for the dialogs.

| Value | Primary Lookup | Secondary Lookup | Fallback |
| :--- | :--- | :--- | :--- |
| **Title** | [Open Graph title](https://ogp.me/) | [Document Title Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/title) | None |
| **Icon** | [Apple touch icon](https://www.computerhope.com/jargon/a/appletou.htm) | [Favicon](https://developer.mozilla.org/en-US/docs/Glossary/Favicon) | A default icon |



