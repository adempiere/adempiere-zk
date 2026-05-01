# adempiere-zk

> [!IMPORTANT]
> This repository is part of the [ADempiere UI Gateway](https://github.com/adempiere/adempiere-ui-gateway) stack —
> a set of cooperating services that together provide the ADempiere web and gRPC frontend.
> The migration of all stack repositories from the [Systemhaus-Westfalia](https://github.com/Systemhaus-Westfalia)
> organization to the [ADempiere](https://github.com/adempiere) organization is currently in progress.
> Until the complete stack has been migrated and validated end-to-end, this repository should be considered
> **under construction**. Interfaces and configuration may change without notice.
> Production use is not yet recommended.

- Docker image for ADempiere ZK UI with customizations.  
- Built on top of [adempiere/zk-ui](https://github.com/adempiere/zk-ui): downloads the published `zk-ui.war` and applies the [adempiere-customizations](https://github.com/adempiere/adempiere-customizations) layer to produce a ready-to-run Docker image.  
- **Published image:** `ghcr.io/adempiere/adempiere-zk`  
- This repository is being created as part of the migration of [Systemhaus-Westfalia/adempiere-ui-gateway](https://github.com/Systemhaus-Westfalia/adempiere-ui-gateway) to the [adempiere](https://github.com/adempiere) GitHub organization.  
- That project is a complete Docker Compose stack running ADempiere with both its traditional ZK UI and a modern Vue-based UI, connected through a gRPC backend, an nginx API gateway, Kafka messaging, and object storage — all configurable from a single environment file and implementing a fully decentralized microservices architecture.  
- This repository, based on [Systemhaus-Westfalia/adempiere-shw-zk](https://github.com/Systemhaus-Westfalia/adempiere-shw-zk), provides the ZK UI container within that stack.  
- This is a temporary README, it will be replaced when the migration will be implemented.

## Status

- Repository under construction.  
- Full source and CI/CD will be added via pull request.  
