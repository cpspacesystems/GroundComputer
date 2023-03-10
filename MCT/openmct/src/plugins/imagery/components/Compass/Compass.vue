/*****************************************************************************
 * Open MCT, Copyright (c) 2014-2022, United States Government
 * as represented by the Administrator of the National Aeronautics and Space
 * Administration. All rights reserved.
 *
 * Open MCT is licensed under the Apache License, Version 2.0 (the
 * "License"); you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 * http://www.apache.org/licenses/LICENSE-2.0.
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
 * WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
 * License for the specific language governing permissions and limitations
 * under the License.
 *
 * Open MCT includes source code licensed under additional open source
 * licenses. See the Open Source Licenses file (LICENSES.md) included with
 * this source code distribution or the Licensing information page available
 * at runtime from the About dialog for additional information.
 *****************************************************************************/

<template>
<div
    class="c-compass"
    :style="`width: 100%; height: 100%`"
>
    <CompassHUD
        v-if="showCompassHUD"
        :sun-heading="sunHeading"
        :camera-angle-of-view="cameraAngleOfView"
        :camera-pan="cameraPan"
    />
    <CompassRose
        v-if="showCompassRose"
        :camera-pan="cameraPan"
        :heading="heading"
        :sized-image-dimensions="sizedImageDimensions"
        :sun-heading="sunHeading"
        :transformations="transformations"
    />
</div>
</template>

<script>
import CompassHUD from './CompassHUD.vue';
import CompassRose from './CompassRose.vue';

export default {
    components: {
        CompassHUD,
        CompassRose
    },
    props: {
        image: {
            type: Object,
            required: true
        },
        sizedImageDimensions: {
            type: Object,
            required: true
        }
    },
    computed: {
        showCompassHUD() {
            return this.hasCameraPan && this.cameraAngleOfView > 0;
        },
        showCompassRose() {
            return (this.hasCameraPan || this.hasHeading) && this.cameraAngleOfView > 0;
        },
        // horizontal rotation from north in degrees
        heading() {
            return this.image.heading;
        },
        hasHeading() {
            return this.heading !== undefined;
        },
        // horizontal rotation from north in degrees
        sunHeading() {
            return this.image.sunOrientation;
        },
        // horizontal rotation from north in degrees
        cameraPan() {
            return this.image.cameraPan;
        },
        hasCameraPan() {
            return this.cameraPan !== undefined;
        },
        cameraAngleOfView() {
            return this.transformations?.cameraAngleOfView;
        },
        transformations() {
            return this.image.transformations;
        }
    },
    methods: {
        toggleLockCompass() {
            this.$emit('toggle-lock-compass');
        }
    }
};
</script>
