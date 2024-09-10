---
tags: 
- Ionic
---

# Routing

> leave all routing in the same component, do not create child IonRouterOutlet because cause bugs on  pages transition animation

```jsx
const MainOutlet: React.FC = () => {
  const { HOME, SETTINGS } = MAIN_ROUTES;
  const { PWA, PROFILE, GENERAL } = SETTINGS_ROUTES;
  const { MAIN_MENU_ID } = C_ROUTES;

  return (
    <IonReactRouter>
      <IonSplitPane contentId={MAIN_MENU_ID}>
        <IsoMainMenu />
        <IonRouterOutlet animated={false} id={MAIN_MENU_ID}>
          <Route path={`/${HOME}`} component={Home} exact={true} />
          <Redirect exact={true} from='/' to={`/${HOME}`} />
          <Route path={`/${SETTINGS}/${GENERAL}`} exact={true} component={Settings} />
          <Route path={`/${SETTINGS}/${PWA}`} exact={true} component={SettingPWA} />
          <Route path={`/${SETTINGS}/${PROFILE}`} exact={true} component={Profile} />
          <Route component={NotFound} />
        </IonRouterOutlet>
      </IonSplitPane>
    </IonReactRouter>
  );
};

  

export default MainOutlet;
```

---