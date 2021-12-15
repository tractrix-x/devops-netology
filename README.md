# devops-netology
Для выполнения заданий в этом разделе давайте склонируем репозиторий с исходным кодом терраформа https://github.com/hashicorp/terraform
В виде результата напишите текстом ответы на вопросы и каким образом эти ответы были получены.

1. Найдите полный хеш и комментарий коммита, хеш которого начинается на aefea.
$ git show aefea
aefead2207ef7e2aa5dc81a34aedf0cad4c32545

$ git show aefea
commit aefead2207ef7e2aa5dc81a34aedf0cad4c32545
Author: Alisdair McDiarmid <alisdair@users.noreply.github.com>
Date:   Thu Jun 18 10:29:58 2020 -0400

    Update CHANGELOG.md

diff --git a/CHANGELOG.md b/CHANGELOG.md
index 86d70e3e0..588d807b1 100644
--- a/CHANGELOG.md
+++ b/CHANGELOG.md
@@ -27,6 +27,7 @@ BUG FIXES:
 * backend/s3: Prefer AWS shared configuration over EC2 metadata credentials by default ([#25134](https://github.com/hashicorp/terraform/issues/25134))
 * backend/s3: Prefer ECS credentials over EC2 metadata credentials by default ([#25134](https://github.com/hashicorp/terraform/issues/25134))
 * backend/s3: Remove hardcoded AWS Provider messaging ([#25134](https://github.com/hashicorp/terraform/issues/25134))
+* command: Fix bug with global `-v`/`-version`/`--version` flags introduced in 0.13.0beta2 [GH-25277]
 * command/0.13upgrade: Fix `0.13upgrade` usage help text to include options ([#25127](https://github.com/hashicorp/terraform/issues/25127))
 * command/0.13upgrade: Do not add source for builtin provider ([#25215](https://github.com/hashicorp/terraform/issues/25215))
 * command/apply: Fix bug which caused Terraform to silently exit on Windows when using absolute plan path ([#25233](https://github.com/hashicorp/terraform/issues/25233))



2. Какому тегу соответствует коммит 85024d3?
git show 85024d3
tag: v0.12.23

3. Сколько родителей у коммита b8d720? Напишите их хеши.
2 родителя 56cd7859e05c36c06b56d013b55a252d0bb7e158 и 9ea88f22fc6269854151c571162c5bcf958bee2b


git show b8d720^
commit 56cd7859e05c36c06b56d013b55a252d0bb7e158
Merge: 58dcac4b7 ffbcf5581
Author: Chris Griggs <cgriggs@hashicorp.com>
Date:   Mon Jan 13 13:19:09 2020 -0800

    Merge pull request #23857 from hashicorp/cgriggs01-stable

    [cherry-pick]add checkpoint links


git show b8d720^2
commit 9ea88f22fc6269854151c571162c5bcf958bee2b
Author: Chris Griggs <cgriggs@hashicorp.com>
Date:   Tue Jan 21 17:08:06 2020 -0800

    add/update community provider listings

diff --git a/website/docs/providers/type/community-index.html.markdown b/website/docs/providers/type/community-index.html.markdown
index 6119f048f..675059dd4 100644
--- a/website/docs/providers/type/community-index.html.markdown
+++ b/website/docs/providers/type/community-index.html.markdown
@@ -28,19 +28,23 @@ please fill out this [community providers form](https://docs.google.com/forms/d/
 - [Apigee](https://github.com/zambien/terraform-provider-apigee)
 - [Artifactory](https://github.com/atlassian/terraform-provider-artifactory)
 - [Auth](https://github.com/Shuttl-Tech/terraform-provider-auth)
-- [Auth0](https://github.com/bocodigitalmedia/terraform-provider-auth0)
+- [Auth0](https://github.com/alexkappa/terraform-provider-auth0)
 - [Automic Continuous Delivery](https://github.com/Automic/terraform-provider-cda)
 - [AVI](https://github.com/avinetworks/terraform-provider-avi)
 - [Aviatrix](https://github.com/AviatrixSystems/terraform-provider-aviatrix)
 - [AWX](https://github.com/mauromedda/terraform-provider-awx)
 - [Azure Devops](https://github.com/agarciamiravet/terraform-provider-azuredevops)
 - [Bitbucket Server](https://github.com/gavinbunney/terraform-provider-bitbucketserver)
+- [CDS](https://github.com/capitalonline/terraform-provider-cds)
 - [Centreon](https://github.com/smutel/terraform-provider-centreon)
 - [Checkly](https://github.com/bitfield/terraform-provider-checkly)
 - [Cherry Servers](https://github.com/cherryservers/terraform-provider-cherryservers)
 - [Citrix ADC](https://github.com/citrix/terraform-provider-citrixadc)
 - [Cloud Foundry](https://github.com/cloudfoundry-community/terraform-provider-cf)
+- [Cloud.dk](https://github.com/danitso/terraform-provider-clouddk)
+- [Cloudability](https://github.com/skyscrapr/terraform-provider-cloudability)
 - [CloudAMQP](https://github.com/cloudamqp/terraform-provider)
+- [Cloudforms](https://github.com/GSLabDev/terraform-provider-cloudforms)
 - [CloudKarafka](https://github.com/cloudkarafka/terraform-provider)
 - [CloudMQTT](https://github.com/cloudmqtt/terraform-provider)
 - [CloudPassage Halo](https://gitlab.com/kiwicom/terraform-provider-cphalo)
@@ -78,6 +82,8 @@ please fill out this [community providers form](https://docs.google.com/forms/d/
 - [Google Calendar](https://github.com/sethvargo/terraform-provider-googlecalendar)
 - [Google G Suite](https://github.com/DeviaVir/terraform-provider-gsuite)
 - [GorillaStack](https://github.com/GorillaStack/terraform-provider-gorillastack)
+- [Greylog](https://github.com/suzuki-shunsuke/go-graylog)
+- [Harbor](https://github.com/BESTSELLER/terraform-harbor-provider)
 - [Hiera](https://github.com/ribbybibby/terraform-provider-hiera)
 - [HPE OneView](https://github.com/HewlettPackard/terraform-provider-oneview)
 - [HTTP File Upload](https://github.com/GSLabDev/terraform-provider-httpfileupload)
@@ -85,6 +91,8 @@ please fill out this [community providers form](https://docs.google.com/forms/d/
 - [IIJ GIO](https://github.com/iij/terraform-provider-p2pub)
 - [Infoblox](https://github.com/hiscox/terraform-provider-infoblox)
 - [InsightOPS](https://github.com/Tweddle-SE-Team/terraform-provider-insight)
+- [Instana](https://github.com/gessnerfl/terraform-provider-instana)
...

bil@LAPTOP-GJ376PE7:/mnt/d/devops-netology/terraform/terraform$ git show b8d720^3
fatal: ambiguous argument 'b8d720^3': unknown revision or path not in the working tree.
Use '--' to separate paths from revisions, like this:
'git <command> [<revision>...] -- [<file>...]'

4. Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами v0.12.23 и v0.12.24.
git log --oneline v0.12.23  v0.12.24

33ff1c03b (tag: v0.12.24) v0.12.24
b14b74c49 [Website] vmc provider links
3f235065b Update CHANGELOG.md
6ae64e247 registry: Fix panic when server is unreachable
5c619ca1b website: Remove links to the getting started guide's old location
06275647e Update CHANGELOG.md
d5f9411f5 command: Fix bug when using terraform login on Windows
4b6d06cc5 Update CHANGELOG.md
dd01a3507 Update CHANGELOG.md
225466bc3 Cleanup after v0.12.23 release
85024d310 (tag: v0.12.23) v0.12.23
4703cb6c1 Update CHANGELOG.md
0b4470e0d Cleanup after v0.12.22 release
18bfd096b (tag: v0.12.22) v0.12.22
c66cdcf78 backend/plan: Show warnings even without changes (backport) (#24172)
566be7a3c Update CHANGELOG.md
d7a9ebf55 Update CHANGELOG.md
791ebcb8e state mv should always target instance each mode
c32ff5ec5 Update CHANGELOG.md
64f328c6a Update CHANGELOG.md
2cdfa0851 registry: configurable client timeout (#24259)
f2800851c Update CHANGELOG.md
ca2facfd9 registry: fix env test cleanup
c8b8bc3f6 registry: setup client logger
8cbdddc21 website/docs/commands: document TF_REGISTRY_DISCOVERY_RETRY
46ec259fa registry: configurable retry client
eb07dccd0 Merge pull request #24176 from hashicorp/cgriggs01-stable-quorum
0a32eab6c Merge pull request #24268 from hashicorp/cgriggs01-stable-oktaasa
adfe8d1e0 Merge pull request #20260 from nlamirault/patch-1
652774430 Update CHANGELOG.md
173530d89 (tag: v0.12.21) v0.12.21
266ba3a0a Update CHANGELOG.md
8fd9d7696 [Website] Adding community providers
7c082b034 website: add token setup callout to remote backend docs (#24109)
1b6ca2884 add Baidu links + okta
1025b285a website: Private registry is free now
477203f01 Update CHANGELOG.md
b6d767a5c terraform: Add test coverage for eval_for_each
257099324 terraform: detect null values in for_each sets
00b9f2291 command/login: Fix browser launcher for WSL users
049f7bf95 Update CHANGELOG.md
c5f181ccf command: Fix stale lock when exiting early
8c19ed71c Update CHANGELOG.md
9f5d3832f Update CHANGELOG.md
15420a759 Update CHANGELOG.md
5a503e292 backend/cos: Add TencentCloud backend cos with lock (#22540)
fb7def460 Update CHANGELOG.md
86155e1c1 command/workspace delete: release lock after workspace removal warning (#24085)
e4809d6d8 Update CHANGELOG.md

5. Найдите коммит в котором была создана функция func providerSource, ее определение в коде выглядит так func providerSource(...) (вместо троеточего перечислены аргументы).
5e06e39fcc86bb622b962c87da84213d3331ddf8


git log -SproviderSource --oneline

5b266dd5c command: Remove the experimental "terraform add" command
c587384df cli: Restore -lock and -lock-timeout init flags
583859e51 commands: `terraform add` (#28874)
5f30efe85 command tests: plan and init (#28616)
c89004d22 core: Add sensitive provider attrs to JSON plan
31a5aa187 command/init: Add a new flag `-lockfile=readonly` (#27630)
bab497912 command/init: Remove the warnings about the "legacy" cache directory
e70ab09bf command: new cache directory .terraform/providers for providers
b3f5c7f1e command/init: Read, respect, and update provider dependency locks
0b734a280 command: Make provider installation interruptible
9f824c53a command: Better in-house provider install errors
d8e996436 terraform: Eval module call arguments for import
87d1fb400 command/init: Display provider validation errors
6b3d0ee64 add test for terraform version
dbe139e61 add test for terraform version -json
b611bd720 reproduction test
8b279b6f3 plugin/discovery: Remove dead code
ca4010706 command/init: Better diagnostics for provider 404s
62d826e06 command/init: Use full config for provider reqs
ae98bd12a command: Rework 0.13upgrade sub-command
5af1e6234 main: Honor explicit provider_installation CLI config when present
269d51148 command/providers: refactor with new provider types and functions
8c928e835 main: Consult local directories as potential mirrors of providers
958ea4f7d internal/providercache: Handle built-in providers
de6c9ccec command/init: Move "vendored provider" test to e2etests
0af09b23c command: apply and most of import tests passing
add7006de command: Fix TestInit_pluginDirProviders and _pluginDirProvidersDoesNotGet
d40085f37 command: Make the tests compile again
3b0b29ef5 command: Add scaffold for 0.13upgrade command
18dd1bb4d Mildwonkey/tfconfig upgrade (#23670)
5e06e39fc Use registry alias to fetch providers

git show 5e06e39fc
commit 5e06e39fcc86bb622b962c87da84213d3331ddf8
Author: findkim <kngo@hashicorp.com>
Date:   Wed Nov 28 10:26:16 2018 -0600

    Use registry alias to fetch providers

diff --git a/plugin/discovery/get.go b/plugin/discovery/get.go
index 2f6ac1a91..751844e17 100644
--- a/plugin/discovery/get.go
+++ b/plugin/discovery/get.go
@@ -134,6 +134,7 @@ func (i *ProviderInstaller) Get(provider string, req Constraints) (PluginMeta, e
        if len(allVersions.Versions) == 0 {
                return PluginMeta{}, ErrorNoSuitableVersion
        }
+       providerSource := allVersions.ID

        // Filter the list of plugin versions to those which meet the version constraints
        versions := allowedVersions(allVersions, req)
@@ -175,7 +176,7 @@ func (i *ProviderInstaller) Get(provider string, req Constraints) (PluginMeta, e
                return PluginMeta{}, ErrorNoVersionCompatibleWithPlatform
        }

-       downloadURLs, err := i.listProviderDownloadURLs(provider, versionMeta.Version)
+       downloadURLs, err := i.listProviderDownloadURLs(providerSource, versionMeta.Version)
        providerURL := downloadURLs.DownloadURL

        i.Ui.Info(fmt.Sprintf("- Downloading plugin for provider %q (%s)...", provider, versionMeta.Version))
@@ -193,6 +194,9 @@ func (i *ProviderInstaller) Get(provider string, req Constraints) (PluginMeta, e
                }
        }

+       printedProviderName := fmt.Sprintf("%s (%s)", provider, providerSource)
+       i.Ui.Info(fmt.Sprintf("- Downloading plugin for provider %q (%s)...", printedProviderName, versionMeta.Version))
+       log.Printf("[DEBUG] getting provider %q version %q", printedProviderName, versionMeta.Version)
        err = i.install(provider, v, providerURL)
        if err != nil {
                return PluginMeta{}, err
diff --git a/plugin/discovery/get_test.go b/plugin/discovery/get_test.go
index 534a01fa5..73e8bdd18 100644
--- a/plugin/discovery/get_test.go
+++ b/plugin/discovery/get_test.go
@@ -130,6 +130,7 @@ func testHandler(w http.ResponseWriter, r *http.Request) {

 func testReleaseServer() *httptest.Server {
        handler := http.NewServeMux()
+       handler.HandleFunc("/v1/providers/-/", testHandler)
        handler.HandleFunc("/v1/providers/terraform-providers/", testHandler)
        handler.HandleFunc("/terraform-provider-template/", testChecksumHandler)
        handler.HandleFunc("/terraform-provider-badsig/", testChecksumHandler)
:



6. Найдите все коммиты в которых была изменена функция globalPluginDirs.

git log -SglobalPluginDirs --oneline
35a058fb3 main: configure credentials from the CLI config file
c0b176109 prevent log output during init
8364383c3 Push plugin discovery down into command package


7. Кто автор функции synchronizedWriters?

git log -SsynchronizedWriters --oneline
bdfea50cc remove unused
fd4f7eb0b remove prefixed io
5ac311e2a main: synchronize writes to VT100-faker on Windows

git show 5ac311e2a
commit 5ac311e2a91e381e2f52234668b49ba670aa0fe5
Author: Martin Atkins <mart@degeneration.co.uk>
Date:   Wed May 3 16:25:41 2017 -0700

