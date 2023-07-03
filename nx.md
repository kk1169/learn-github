# Create Project with frontend and api
```
npx create-nx-workspace@latest wd-quiz
ng g @nrwl/angular:application fe

nx g @nx/angular:lib wd-ui --prefix wd --directory=shared
nx g @nx/angular:component button --project=ui --standalone
```

## Adding Tailwind css into ui  library
```
npx tailwindcss init -p
```
## Adding Storybook
```
npm i -D @nx/storybook
nx g storybook-configuration shared-wd-ui

npx nx g @nx/angular:lib my-lib --buildable --add-tailwind
```