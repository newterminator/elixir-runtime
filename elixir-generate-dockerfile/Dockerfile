# Copyright 2017 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.


# Use the Elixir base image for building.
FROM elixir-base

# Build the Dockerfile generation script
COPY src/ /src/
RUN cd /src \
    && mix deps.get \
    && mix escript.build


# Use the Elixir base image again for the generator runtime.
FROM elixir-base

ARG debian_image=elixir-debian
ARG base_image=elixir-base
ARG builder_image=elixir-builder

# Install the wrapper script and templates.
COPY app/ /app/

# Copy the built generate_dockerfile escript
COPY --from=0 /src/generate_dockerfile /app/generate_dockerfile

ENV ELIXIR_DEBIAN_IMAGE=${debian_image} \
    ELIXIR_BASE_IMAGE=${base_image} \
    ELIXIR_BUILDER_IMAGE=${builder_image}

# The entry point runs the generation script.
ENTRYPOINT ["/app/generate_dockerfile.sh"]
