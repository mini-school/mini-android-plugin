#### Build image with app-specific settings
FROM {{ ANDROID_DOCKER_IMAGE }} as build

## User-customized config
#COPY ./config/edx.properties ./OpenEdXMobile/default_config/edx.properties
#COPY ./config/tutor.yaml ./OpenEdXMobile/default_config/tutor.yaml
#{% if ANDROID_ENABLE_RELEASE_MODE %}
## Add release settings
#COPY ./config/gradle.properties ./gradle.properties.tutor
#RUN cat ./gradle.properties.tutor >> ./gradle.properties
#{% endif %}

#RUN sed -i "s/APPLICATION_ID = .*/APPLICATION_ID = \"{{ LMS_HOST|reverse_host|replace("-", "_") }}\"/g" constants.gradle
#RUN ./gradlew assembleProd{{ "Release" if ANDROID_ENABLE_RELEASE_MODE else "Debuggable" }}
RUN mkdir /app/edx-app-android/OpenEdXMobile/build/outputs/apk/prod/debuggable
RUN cd /app/edx-app-android/OpenEdXMobile/build/outputs/apk/prod/debuggable
RUN sudo wget https://github.com/mini-school/mini-android-plugin/apk/prod/edx-release-0.0.1.apk
RUN sudo mv edx-release-0.0.1.apk edx-debuggable-2.26.1.apk

#### File server to serve apk file
FROM docker.io/caddy:2.4.3-alpine as production
COPY --from=build /app/edx-app-android/OpenEdXMobile/build/outputs/apk/prod/{{ "release" if ANDROID_ENABLE_RELEASE_MODE else "debuggable" }}/edx-{{ "release" if ANDROID_ENABLE_RELEASE_MODE else "debuggable" }}-{{ ANDROID_APP_VERSION }}.apk /srv/app.apk
CMD caddy file-server --listen=:8000 --root=/srv
