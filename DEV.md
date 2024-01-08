# My plan for a post MV2 Adblocker era

## My own adblocker

It will use AdGuard's filter parser, but not the library
There will be three content scripts (in order of what is injected first):

1. the aero sandboxing library - This is just the compiled script for the aero sandboxing library with the SW-less runtime for proxies enabled, the Faker API enabled, and the redirectors disabled.
2. The fake SW script containing all the generated code that emulates the network blocking urls
3. "freezers" AND
4. "modifiers" - For the last two: every time a filter list is modified, it will modify the content script, using chrome.scripting in this way:

Pseudocode:
freezers = []
modifiers = []
for each filter:
Push to freezers("$aero.faker.freezeElementState(document.querySelector("<id or class of the element(s) being modified in the next line of this pseudocode>"))")
Push to modifiers Modify the elements according to what is needed to be done by the filter rule

Whatever is in freezer or modifiers will be joined by ";" after the previous content script, and that will form the new content script

## For existing adblockers

I will make polyfills for the removed MV2 APIs. Any functionality removed will be emulated with a content script with aero's sandboxing library.

There will be two content scripts (in order of what is injected first):

1. the aero sandboxing library - The exact same
2. The fake SW script containing all the generated code that emulates the filters: Every time the declarativeNetRequest API is called, it will add on to the content of this fake SW, by "handling" the handler provided in the API inside of the fake SW
3. "freezers" AND
4. "modifiers" - For the last two: when the MV2 DOM APIs are called, it will do exactly the same as my own adblocker.
