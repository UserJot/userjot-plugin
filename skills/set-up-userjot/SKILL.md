---
name: set-up-userjot
description: Install the UserJot widget in this app and connect it to the app's signed-in users. Use when asked to add UserJot, a feedback board, feature requests, a roadmap, a changelog, or an in-app support widget.
---

# Set up UserJot in this app

UserJot gives an app one widget for feedback, a roadmap, product updates, a help center, and support chat. This skill installs that widget, ties it to the app's signed-in users, and adds links to it from the app's own menus.

Read the app before changing it. Find where the root layout lives, how the current user is loaded, and where the account or help menu is.

## 1. Get the project ID

The widget needs the workspace's project ID. Ask the person for it. They copy it from their UserJot dashboard, and if they do not have a workspace yet they create one free at https://app.userjot.com/auth/signup.

If the app keeps public configuration in environment variables, store the ID the same way (for example `NEXT_PUBLIC_USERJOT_PROJECT_ID` in a Next.js app) and add it to the example env file. The project ID is public. It is safe in client code.

## 2. Install the snippet

Add this where every page of the app loads it, once. In a plain HTML site that is just before `</body>`. In a framework, use its root layout and its way of adding a script (for example `next/script` in `app/layout.tsx`).

```html
<script>
  window.uj ||= new Proxy(
    {},
    {
      get: (_, p) =>
        p == 'then'
          ? 0
          : (...a) => {
              (window.$ujq ||= []).push([p, ...a]);
            }
    }
  );
  uj.init('YOUR_PROJECT_ID');
</script>
<script type="module" async src="https://cdn.userjot.com/sdk/v3/uj.js"></script>
```

The first script creates `window.uj` and queues calls. The second loads the SDK, which replays them. Calls can be made in any order and before the SDK has loaded.

If the app's content security policy blocks inline scripts, use the single tag instead:

```html
<script
  type="module"
  async
  src="https://cdn.userjot.com/sdk/v3/uj.js"
  data-project="YOUR_PROJECT_ID"
></script>
```

In a TypeScript app, declare `window.uj` in the app's global types so calls to it compile.

## 3. Identify signed-in users

Without this every visitor is anonymous. Call `identify` once the app knows who the user is, and `logout` when they sign out.

```javascript
if (user) {
  uj.identify({
    user: {
      id: user.id,
      email: user.email,
      firstName: user.firstName,
      lastName: user.lastName
    }
  });
} else {
  uj.logout();
}
```

`user.id` is required and must be a stable ID from the app's own database, never an email or display name. Send `email` whenever the app has it.

For a B2B app, pass the user's company too:

```javascript
uj.identify({
  user: { id: user.id, email: user.email },
  companies: [{ id: company.id, name: company.name }]
});
```

## 4. Add links from the app's own menus

People open the widget far more often from a menu item they already use than from a floating button. Add these to the account, help, or sidebar menu, matching the labels and style of the items already there:

```javascript
// "Give feedback"
uj.open({ to: 'feedback', compose: true });

// "What's new"
uj.open({ to: 'updates' });

// "Roadmap"
uj.open({ to: 'roadmap' });

// "Help", only if the workspace's help center is live
uj.open({ to: 'help' });

// "Contact support", only if Conversations is turned on in the workspace
uj.open({ to: 'messages' });
```

Ask the person which of these they want before adding them. If they want only their own menu items and no floating button, hide the default launcher:

```javascript
uj.init('YOUR_PROJECT_ID', { widget: { launcher: false } });
```

## 5. Check it works

Run the app and load a page.

1. The launcher appears in the bottom right corner, unless it was hidden in step 4.
2. Opening it shows feedback, the roadmap and updates.
3. Signed in as a test user, `uj.getState().session.status` is `identified`.
4. The browser console has no UserJot warnings. A warning that calls are pending means `uj.init` never ran.

Tell the person what was added and where, and which menu items now open the widget.

## More

The full SDK is documented at https://feedback.userjot.com/help. The index at https://feedback.userjot.com/help/llms.txt lists every article, and each link in it returns markdown. Read the relevant article before using anything not shown here, such as signed identity, inline embeds, or the Swift SDK.

This plugin also connects the UserJot MCP server. Once the person has signed in with `/mcp`, its tools can read and change their workspace: requests, votes, comments, the changelog, conversations, and help articles.
