---
title: Build lists | ABF API
---

# Build lists API

* [Get a single build list](#get-a-single-build-list)
* [Create build list](#create-build-list)
* [Cancel build list](#cancel-build-list)
* [Publish build list](#publish-build-list)
* [Publish build list into testing](#publish-build-list-into-testing)
* [Rerun tests](#rerun-tests)
* [Create container](#create-container)
* [Reject publish build list](#reject-publish-build-list)
* [List build lists](#list-build-lists)
* [List build lists for a project](#list-build-lists-for-a-project)
* [Destroy build list](#destroy-build-list)

## Get a single build list
  Block "Logs" is available only for new core.<br/>
  The value of parameter "url" in block "Packages" is empty for old core.

    GET /api/v1/build_lists/:id.json

### Available statuses for parameter "container_status":
* `4000` — waiting for request for publishing container;
* `6000` — container has been published;
* `7000` — container is being published;
* `8000` — publishing error.

### Parameters:
id
: _Integer_ identifier of current build list

### Response:

<%= json(:build_list_show_parameters) %>

### Example:

<%= json(:build_list_show_example) %>

## Create build list

Create new build list for project.

    POST /api/v1/build_lists.json

### Input:

project_id
: _Required_ **integer** — Identifier project for which need to run assembly.

commit_hash
: _Required_ **string** — SHA of project commit for which need to run assembly.

update_type
: _Required_ **string** — Informing customers about the priority and character of updates: `security`, `bugfix`, `enhancement`,`recommended` or `newpackage`.

save_to_repository_id
: _Required_ **integer** — Repository identifier for package storage. List of repositories can be find in api call:
[see repositories section](/abf/api/v1/projects/#get-a-single-project).

build_for_platform_id
: _Required_ **integer** — Identifier of platform for which need to run assembly. List of platforms can be find in api call:
[list of platforms](/abf/api/v1/platforms/#list-of-platforms-for-which-you-can-create-build-list).

include_repos
: _Required_ **array** of **integers** — Repositories to connect for building this build list. Repositories must belong to platform(build_for_platform_id) for which performed assembly. Use `extra_repositories` for connect repositories from personal platforms.

arch_id
: _Required_ **integer** — Identifier architecture for which need to run assembly.

auto_publish_status
: _Optional_ **string** — `default` to enable automatic publication build list if the build succeeds, `testing` to enable automatic publication build list into `testing` sub-repository if the build succeeds, `none` allow manually publication. If in repository for package storage disabled publication without QA, parameter auto_publish_status automatically will be set to `none`.

auto_create_container
: _Optional_ **boolean** — `true` to enable automatic creating container of build list if the build succeeds, `false` allow manually creating. Default value: `false`.

extra_repositories
: _Optional_ **array** of **integers** — Repositories to connect for building this build list. Available only if `save_to_repository` - personal repository.

extra_build_lists
: _Optional_ **array** of **integers** — Build lists with containers (`container_status` should be `6000`) to connect for building this build list.<br/>
For main platform you can connect only build lists which have been saved into same platform. <br/>
Only build lists with the same architecture will be connected or oriented to the both architectures (the property `publish_i686_into_x86_64` (only for `rhel`) in the settings of project is `true`).

use_cached_chroot
: _Optional_ **boolean** — `true` to use a cached chroot for building of build list. Default value: `false`.

use_extra_tests
: _Optional_ **boolean** — `true` to use a more complex testing of packages. Default value: `true`.

save_buildroot
: _Optional_ **boolean** — `true` to save a RPM buildroot if build will fail. Default value: `false`.

include_testing_subrepository
: _Optional_ **boolean** — `true` to connect `testing` subrepository for building this build list. Default value: `false`.

external_nodes
: _Optional_ **string** — `owned` to use own external ABF node, `everything` to use any external ABF node.

### Request

<%= json(:build_list_create_parameters) %>

### Request example:

<%= json(:build_list_create_example) %>

### Response:

<%= json(:build_list_create_response) %>

### Response example:

<%= json(:build_list_create_response_example) %>

## Cancel build list

By this request you can cancel build list.
Only build list with status build pending (2000) or build started (3000) can be canceled.

    PUT /api/v1/build_lists/:id/cancel.json

### Parameters:
id
: _Integer_ identifier of current build list

### Response:

<%= json(:build_list_cancel_response) %>

### Example:

<%= json(:build_list_cancel_response_example) %>

&nbsp;

<%= json(:build_list_cancel_response_example2) %>

## Publish build list

By this request you can publish build list.
Only build list with status:

* build complete (0),
* publishing error (8000),
* tests failed (11000),
* build has been published into testing (12000),
* publishing error into testing (14000)

can be published.<br/>
Admin of platform/repository has access to publish build list again with status published (6000).<br/>
Be careful: secondary publication will be able to break relationships in the repository!<br/>
All extra build lists should be published before publishing this build list!

    PUT /api/v1/build_lists/:id/publish.json

### Parameters:
id
: _Integer_ identifier of current build list

### Response:

<%= json(:build_list_publish_response) %>

### Example:

<%= json(:build_list_publish_response_example) %>

&nbsp;

<%= json(:build_list_publish_response_example2) %>

## Publish build list into testing

By this request you can publish build list into `testing` subrepository.
Only build list with status:

* build complete (0),
* publishing error (8000),
* tests failed (11000),
* build has been published into testing (12000),
* publishing error into testing (14000)

can be published.<br/>

    PUT /api/v1/build_lists/:id/publish_into_testing.json

### Parameters:
id
: _Integer_ identifier of current build list

### Response:

<%= json(:build_list_publish_response) %>

### Example:

<%= json(:build_list_publish_response_example) %>

&nbsp;

<%= json(:build_list_publish_response_example2) %>

## Rerun tests

By this request you can rerun tests.
Tests can be rerun only for build list with statuses build complete (0), tests failed (11000).

    PUT /api/v1/build_lists/:id/rerun_tests.json

### Parameters:
id
: _Integer_ identifier of current build list

### Response:

<%= json(:build_list_rerun_tests_response) %>

### Example:

<%= json(:build_list_rerun_tests_response_example) %>

&nbsp;

<%= json(:build_list_rerun_tests_response_example2) %>

## Create container

By this request you can create container.
Container can be created only for build list with statuses build complete (0), build published (6000), build publish (7000), publishing error (8000), publishing rejected (9000), tests failed (11000).

    PUT /api/v1/build_lists/:id/create_container.json

### Parameters:
id
: _Integer_ identifier of current build list

### Response:

<%= json(:build_list_create_container_response) %>

### Example:

<%= json(:build_list_create_container_response_example) %>

&nbsp;

<%= json(:build_list_create_container_response_example2) %>

## Reject publish build list

By this request you can reject publish build list.
Only build list with status:

* build complete (0),
* publishing error (8000),
* tests failed (11000),
* build has been published into testing (12000),
* publishing error into testing (14000)

can be rejected.

    PUT /api/v1/build_lists/:id/reject_publish.json

### Parameters:
id
: _Integer_ identifier of current build list

### Response:

<%= json(:build_list_reject_response) %>

### Example:

<%= json(:build_list_reject_response_example) %>

&nbsp;

<%= json(:build_list_reject_response_example2) %>

## List build lists

By this way you can search build list you need.

    GET /api/v1/build_lists.json?<search params>

### Parameters:

page
: _Optional_ **Integer** - page number of build lists results list.

per_page
: _Optional_ **Integer** - amount of build list per one page. Default 20, maximum 100.

filter[status]
: _Optional_ **integer** - code of the build status
:   * `0`     — build complete;
    * `1`     — platform not found;
    * `2`     — platform pending;
    * `3`     — project not found;
    * `4`     — project version not found;
    * `666`   — build error;
    * `2000`  — build pending;
    * `2500`  — rerun tests;
    * `2550`  — build is being rerun tests;
    * `3000`  — build started;
    * `4000`  — waiting for response;
    * `5000`  — build canceled;
    * `6000`  — build has been published;
    * `7000`  — build is being published;
    * `8000`  — publishing error;
    * `9000`  — publishing rejected;
    * `10000` — build is canceling;
    * `11000` — tests failed;
    * `12000` — build has been published into testing;
    * `13000` — build is being published into testing;
    * `14000` — publishing error into testing.

filter[arch_id]
: _Optional_ **integer** - identifier of the architecture.

filter[project_name]
: _Optional_ **string** — project name.

filter[updated_at_start / updated_at_end]
: _Optional_ **unixtime** - start and end of the build list last change date diapason.

filter[ownership]
: _Optional_ `owned`, `related` or `index` - ownership type. Default: `owned`.

filter[mass_build_id]
: _Optional_ **integer** — mass build identifier.

filter[save_to_platform_id]
: _Optional_ **integer** - platform id for build save.

filter[build_for_platform_id]
: _Optional_ **integer** - platform id for build.

filter[save_to_repository_id]
: _Optional_ **integer** - repository id for package storage.

### Response:

<%= json(:build_list_search_response) %>

### Example of request url:

> /api/v1/build_lists.json?filter[project_name]=rails

> /api/v1/build_lists.json?filter[ownership]=owned&filter[status]=6000&filter[arch_id]=2

### Examples of responses:

<%= json(:build_list_search_response_example) %>

## List build lists for a project

    GET /api/v1/projects/:project_id/build_lists.json?<search params>

### Parameters:

Look at [List build lists](#list-build-lists)

### Response:

<%= json(:build_list_search_response) %>

### Example of request url:

> /api/v1/projects/42/build_lists.json?filter[status]=6000

### Examples of responses:

<%= json(:build_list_search_response_example) %>


## Destroy build list

You can't destroy build list. Only cancel it. :)

