---
tags: ["docker", "docker compose", "yaml", "version"]
description: "WARN: docker-compose.yml: `version` is obsolete"
title: "How to Solve WARN: docker-compose.yml: `version` is Obsolete"
---

# How to Solve the WARN: docker-compose.yml: `version` is Obsolete Warning

When working with Docker Compose, you may encounter the warning message: `WARN: docker-compose.yml: 'version' is obsolete`. This warning indicates that the `version` field in your `docker-compose.yml` file is no longer necessary with the latest versions of Docker Compose. In this guide, we’ll explain why this warning appears and how to update your `docker-compose.yml` file to resolve it.

## Why the Warning Appears

In earlier versions of Docker Compose, the `version` field was required to specify the version of the Compose file format being used. This helped Docker Compose understand how to parse and interpret the file. However, with the release of Docker Compose v1.27.0, the tool no longer requires the `version` field. Instead, it automatically determines the appropriate version based on the syntax and features used in the file.

## Steps to Resolve the Warning

Here’s a step-by-step guide to update your `docker-compose.yml` file and remove the obsolete `version` field:

### 1. Open Your `docker-compose.yml` File

Open your `docker-compose.yml` file in your preferred text editor. It might look something like this:

```yaml
version: '3.8'
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
  db:
    image: postgres:latest
    environment:
      POSTGRES_PASSWORD: example
```

### 2. Remove the `version` Field

Simply delete the `version` line from your file. Your updated `docker-compose.yml` file should look like this:

```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
  db:
    image: postgres:latest
    environment:
      POSTGRES_PASSWORD: example
```

### 3. Save the File

Save the changes to your `docker-compose.yml` file.

### 4. Verify the Changes

To ensure everything works correctly, run the following command in your terminal:

```sh
docker-compose up
```

Docker Compose should start your services without displaying the `version` warning.

## Additional Tips

- **Check Compatibility**: Before removing the `version` field, ensure your Docker Compose version supports this change. This feature is available from Docker Compose v1.27.0 and later.
- **Update Docker**: If you encounter issues, consider updating Docker and Docker Compose to the latest versions to take advantage of new features and improvements.

## Conclusion

By following these steps, you can resolve the `WARN: docker-compose.yml: 'version' is obsolete` warning and streamline your Docker Compose configuration. Removing the `version` field makes your `docker-compose.yml` file cleaner and ensures compatibility with the latest Docker Compose versions.

For more information and updates, refer to the official [Docker Compose documentation](https://docs.docker.com/compose/compose-file/).

---

Now that you have updated your `docker-compose.yml` file, you can continue developing your applications with fewer warnings and a more streamlined configuration.

---

This guide provided a comprehensive solution to the warning issue. If you have any further questions or need additional assistance, feel free to reach out.
