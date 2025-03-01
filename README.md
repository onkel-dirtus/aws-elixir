# AWS clients for Elixir

[![Actions Status](https://github.com/aws-beam/aws-elixir/workflows/Build/badge.svg)](https://github.com/aws-beam/aws-elixir/actions)
[![Module Version](https://img.shields.io/hexpm/v/aws.svg)](https://hex.pm/packages/aws)
[![Hex Docs](https://img.shields.io/badge/hex-docs-lightgreen.svg)](https://hexdocs.pm/aws/)
[![Total Download](https://img.shields.io/hexpm/dt/aws.svg)](https://hex.pm/packages/aws)
[![License](https://img.shields.io/hexpm/l/aws.svg)](https://github.com/aws-beam/aws-elixir/blob/master/LICENSE.md)
[![Last Updated](https://img.shields.io/github/last-commit/aws-beam/aws-elixir.svg)](https://github.com/aws-beam/aws-elixir/commits/master)

🌳 With this library you can have access to almost all AWS services without hassle. ⚡

## Features

* A clean API separated per service. Each module has a correspondent service.
* Support for most of AWS services.
* Configurable HTTP client and JSON parser.
* Generated by [aws-codegen](https://github.com/aws-beam/aws-codegen) using the
  same JSON descriptions of AWS services used to build the
  [AWS SDK for Go](https://github.com/aws/aws-sdk-go/tree/master/models/apis).
* Documentation is updated from the official AWS docs.

## Usage

Here is an example of listing Amazon Kinesis streams. First you need to start a
console with `iex -S mix`.

After that, type the following:

```elixir
iex> client = AWS.Client.create("your-access-key-id", "your-secret-access-key", "us-east-1")
iex> {:ok, result, resp} = AWS.Kinesis.list_streams(client, %{})
iex> IO.inspect(result)
%{"HasMoreStreams" => false, "StreamNames" => []}
```

If you are using `Amazon.S3`, you can upload a file with integrity check doing:

```elixir
iex> client = AWS.Client.create("your-access-key-id", "your-secret-access-key", "us-east-1")
iex> file =  File.read!("./tmp/your-file.txt")
iex> md5 = :crypto.hash(:md5, file) |> Base.encode64()
iex> AWS.S3.put_object(client, "your-bucket-name", "foo/your-file-on-s3.txt",
  %{"Body" => file, "ContentMD5" => md5})
```

Note that you may need to specify the `ContentType` attribute when calling `AWS.S3.put_object/4`.
This is because S3 will use that to store the MIME type of the file.

Remember to check the operation documentation for details:
[https://docs.aws.amazon.com/](https://docs.aws.amazon.com/)

## Installation

Add `:aws` to your list of dependencies in `mix.exs`. It also requires
[hackney](https://github.com/benoitc/hackney) for the default HTTP client.
Optionally, you can implement your own (Check `AWS.Client` docs).

```elixir
def deps do
  [
    {:aws, "~> 0.8.0"},
    {:hackney, "~> 1.17"}
  ]
end
```

Run `mix deps.get` to install.

## Development

Most of the code is generated using the
[aws-codegen](https://github.com/aws-beam/aws-codegen) library from the JSON
descriptions of AWS services provided by Amazon. They can be found in
`lib/aws/generated`.

Code outside `lib/aws/generated` is manually written and used as support for
the generated code.

## Documentation

Online
* [Hex Docs](https://hexdocs.pm/aws)

Local
* Run `mix docs`
* Open `docs/index.html`

__Note:__ Arguments, errors and response structure can be found by viewing the model schemas used to generate this module at `aws-sdk-go/models/apis/<aws-service>/<version>/`.

An example is [`aws-sdk-go/models/apis/rekognition/2016-06-27/api-2.json`](https://github.com/aws/aws-sdk-go/blob/master/models/apis/rekognition/2016-06-27/api-2.json).
Alternatively you can access the documentation for the service you want at [AWS docs page](https://docs.aws.amazon.com/).

## Tests

```
$ mix test
```

## Release

* Make sure the `CHANGELOG.md` is up-to-date and and reflects the changes for
  the new version.
* Bump the version here in the `README.md` and in `mix.exs`.
* Run `git tag v$VERSION` to tag the version that was just published.
* Run `git push --tags origin master` to push tags to Github.
* Run `mix hex.publish` to publish the new version.

## Copyright & License

Copyright (c) 2015 Jamshed Kakar <jkakar@kakar.ca>

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at [http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
