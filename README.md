# OpenAPI code generator

The generator reads an OpenAPI or Swagger schema supplied by a consumer project
and creates:

- DTO types in `internal/dto/types_generated.go`;
- REST handler skeletons in `internal/api/rest/handlers_generated.go`;
- Swagger JSON and a Swagger UI entry point in `docs/swagger/`.

Run from a consumer project:

```bash
go run github.com/mkrtychanr/dto-generator/cmd/generate@v0.1.0 \
	-schema api/schema.yaml \
	-output . \
	-module "$(go list -m)"
```

The generator is a standalone Go CLI. A consumer project keeps its own schema
and calls a tagged version from its `Makefile`:

```make
GENERATOR_VERSION ?= v0.1.0

.PHONY: generate
generate:
	go run github.com/mkrtychanr/dto-generator/cmd/generate@$(GENERATOR_VERSION) \
		-schema api/schema.yaml \
		-output . \
		-module $$(go list -m)
```

`-schema` is the input OpenAPI/Swagger file, `-output` is the consumer project
root, and `-module` is read from the consumer's `go.mod`. The generated REST
handlers import DTOs using that module path.

Open `docs/swagger/index.html` in a browser to view the generated contract. The
generated handlers are intentionally minimal and are entry points for the
application's router and business logic.
