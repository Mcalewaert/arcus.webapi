---
title: "Replace configuration tokens with ISecretProvider"
layout: default
---

The `Arcus.WebApi.Security` package provides a mechanism to use your own `ISecretProvider` implementation when building your configuration for your web application.

### Usage

When building your IConfiguration, you can use the extension .AddAzureKeyVault to pass in your ISecretProvider instead of using the built-in [Azure Key Vault provider](https://docs.microsoft.com/en-us/aspnet/core/security/key-vault-configuration?view=aspnetcore-2.2#packages).

```csharp
var vaultAuthenticator = new ManagedServiceIdentityAuthenticator();
var vaultConfiguration = new KeyVaultConfiguration(keyVaultUri);
var yourSecretProvider = new KeyVaultSecretProvider(vaultAuthenticator, vaultConfiguration);

var config = new ConfigurationBuilder()
    .AddAzureKeyVault(yourSecretProvider)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```