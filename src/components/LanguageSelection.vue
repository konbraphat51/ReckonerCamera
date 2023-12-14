<template>
	<div id="LanguageSelection">
		{{ t("LanguageSelection.Language") }}

		<select v-model="$i18n.locale">
			<option value="en">English</option>
			<option value="ja">日本語</option>
		</select>
	</div>
</template>

<script>
export default {
	name: "LanguageSelection",
	setup() {
		//set up i18n
		const {t} = VueI18n.useI18n()
		return {t}
	},
}
</script>

<i18n>
    {
        "en": {
            "LanguageSelection": {
                "Language": "Language"
            }
        },
        "ja": {
            "LanguageSelection": {
                "Language": "言語"
            }
        }
    }
</i18n>
