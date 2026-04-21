# Copyright (c) 2023, WSO2 LLC. (https://www.wso2.com/) All Rights Reserved.
#
# WSO2 LLC. licenses this file to you under the Apache License,
# Version 2.0 (the "License"); you may not use this file except
# in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied. See the License for the
# specific language governing permissions and limitations
# under the License.

FROM golang:1.22-alpine AS build-env

# Build-time variable
ARG TEST_BUILD_VAR

RUN mkdir /app
WORKDIR /app

COPY go.mod ./
RUN go mod download

# Create non-root user
RUN addgroup -g 10014 choreo && \
    adduser --disabled-password --no-create-home --uid 10014 --ingroup choreo choreouser

COPY . .

ENV CGO_ENABLED=0 GOOS=linux GOARCH=amd64

RUN echo "Build var is: $TEST_BUILD_VAR"

# Build with updated Go version
RUN go build -a -o /go/bin/app -buildvcs=false

# Minimal runtime image
FROM alpine:3.19

COPY --from=build-env /go/bin/app /go/bin/app

USER 10014
ENTRYPOINT ["/go/bin/app"]
