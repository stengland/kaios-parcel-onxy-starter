<script lang="ts">
  import Dashboard from 'onyx-ui/components/app/Dashboard.svelte';
  import OnyxApp from 'onyx-ui/components/app/OnyxApp.svelte';
  import { Priority } from 'onyx-ui/enums';
  import { KeyManager, Onyx } from 'onyx-ui/services';
  import Router, { location, pop } from 'svelte-spa-router';
  import AppMenu from './components/AppMenu.svelte';
  import Alerts from './routes/Alerts.svelte';
  import AppSettings from './routes/AppSettings.svelte';
  import Buttons from './routes/Buttons.svelte';
  import Cards from './routes/Cards.svelte';
  import ContextMenus from './routes/ContextMenus.svelte';
  import { default as Dialogs, default as Popups } from './routes/Dialogs.svelte';
  import Form from './routes/Form.svelte';
  import Home from './routes/Home.svelte';
  import Lists from './routes/Lists.svelte';
  import Redirect from './routes/Redirect.svelte';
  import Toasts from './routes/Toasts.svelte';
  import Typography from './routes/Typography.svelte';
  import { settings } from './stores/settings';

  const routes = {
    '/': Home,
    '/cards/:cardId': Cards,
    '/typography/:cardId': Typography,
    '/lists/:cardId': Lists,
    '/contextMenus/:cardId': ContextMenus,
    '/buttons/:cardId': Buttons,
    '/forms/:cardId': Form,
    '/alerts/:cardId': Alerts,
    '/dialogs/:cardId': Dialogs,
    '/popups/:cardId': Popups,
    '/toasts/:cardId': Toasts,
    '/settings/:cardId': AppSettings,
    '*': Redirect,
  };

  KeyManager.subscribe(
    {
      onBackspace: () => {
        // If on the main screen, let KaiOS minimize the app
        if ($location === '/') {
          console.log('exit app');
          return false;
        }

        pop();
        return true;
      },
    },
    Priority.Low
  );

  $: Onyx.settings.update($settings);

</script>

<OnyxApp>
  <AppMenu slot="app-menu" />
  <Router {routes} />
  <Dashboard slot="dashboard">Hello</Dashboard>
</OnyxApp>
