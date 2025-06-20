---
description: >-
  When an app is Establishing a Connection or Sending a Transaction, Phantom
  will present the user with the above dialogs. In order to display the title
  and icon for these dialogs, Phantom will inspect
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Displaying Apps within Dialogs

![Connection and transaction approval dialog for mango.markets](<../.gitbook/assets/image (9).png>)

When an app is [Establishing a Connection](../solana/establishing-a-connection.md) or [Sending a Transaction](../solana/sending-a-transaction.md), Phantom will present the user with the above dialogs. In order to display the title and icon for these dialogs, Phantom will inspect the application's HTML for the following items:

| Value     | Primary Lookup                                                         | Secondary Lookup                                                                          | Fallback       |
| --------- | ---------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | -------------- |
| **Title** | [Open Graph title](https://ogp.me/)                                    | [Document Title Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/title) | None           |
| **Icon**  | [Apple touch icon](https://www.computerhope.com/jargon/a/appletou.htm) | [Favicon](https://developer.mozilla.org/en-US/docs/Glossary/Favicon)                      | A default icon |

