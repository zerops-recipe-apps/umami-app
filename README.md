# Zerops x Umami
[Umami](https://umami.is/) makes it easy to collect, analyze, and understand your web data — while maintaining visitor privacy and data ownership. [Zerops](https://zerops.io) makes it easy to deploy Umami.

![unami](https://github.com/zeropsio/recipe-shared-assets/blob/main/covers/svg/recipe-umami.svg)

<br />

## Deploy on Zerops
You can either click the deploy button to deploy directly on Zerops, or manually copy the [import yaml](https://github.com/zeropsio/recipe-unami/blob/main/zerops-project-import.yml) to the import dialog in the Zerops app.

[![Deploy on Zerops](https://github.com/zeropsio/recipe-shared-assets/blob/main/deploy-button/green/deploy-button.svg)](https://app.zerops.io/recipe/unami)

<br/>

## Recipe features
- **Unami** app running on a load balanced **Zerops Node.js** service
- Zerops **PostgreSQL 16** service as database
- Utilization of Zerops' built-in **environment variables** system

<br/>

## Production vs. development

Base of the recipe is ready for production, the difference comes down to:

- Use highly available version of the PostgreSQL database (change `mode` from `NON_HA` to `HA` in recipe YAML, `db` service section)
- Set up database backups in the `db` service details.

<br/>
<br/>

Need help setting your project up? Join [Zerops Discord community](https://discord.com/invite/WDvCZ54).

## Knowledge Base

<!-- #ZEROPS_EXTRACT_START:knowledge-base# -->

### Next.js Standalone Build Artifact

The Umami app is built and deployed in "standalone" mode, see: [output: 'standalone'](https://nextjs.org/docs/pages/api-reference/config/next-config-js/output).
That means the development and maintenance scripts (e.g. `update-tracker`) are not available in the runtime container by default.

To enable this functionality, follow these steps:
1. [SSH into the runtime container](https://docs.zerops.io/references/networking/ssh) or open service web shell in GUI
2. Add some RAM for `pnpm install`:
```shell
zsc scale ram +2GB 10m

# Wait for the scaling to apply.
sleep 10s 
```
3. Install script dependencies:
```shell
pnpm add npm-run-all dotenv chalk semver prisma@6.18.0 @prisma/adapter-pg@6.18.0
```

After that, you should be able to run the maintenance tasks, e.g.:
```shell
pnpm run update-tracker
pnpm run update-db
```

<!-- #ZEROPS_EXTRACT_END:knowledge-base# -->
