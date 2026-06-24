# Thing that I learn

The main motive of this repo is to record the things that I learn in my journey. This are the small skill that I need to remember and store.
This readme is to tell the one line of each thing and they is a dedicated file for each
---
# Kwallet used in Linux
`Simplt workflow`
1.  Application like `VS code `need to store the `user login` details in the `system`
2.  So first `VS code encrypt` the `user detail` and send it to the `Kwallet` through `System API`
3.  The `Kwallet` again encryption data received from the `API` for safety perpose
4.  Then it `store the encrypted` in its `own database` in the `disk`
5.  When the `VS code wants the data` it ask the `Kwallet` then it `decrypt it` and give it to the `VS code`.
