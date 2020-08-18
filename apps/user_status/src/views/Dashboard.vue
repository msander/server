<!--
  - @copyright Copyright (c) 2020 Georg Ehrke <oc.list@georgehrke.com>
  - @author Georg Ehrke <oc.list@georgehrke.com>
  -
  - @license GNU AGPL version 3 or any later version
  -
  - This program is free software: you can redistribute it and/or modify
  - it under the terms of the GNU Affero General Public License as
  - published by the Free Software Foundation, either version 3 of the
  - License, or (at your option) any later version.
  -
  - This program is distributed in the hope that it will be useful,
  - but WITHOUT ANY WARRANTY; without even the implied warranty of
  - MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
  - GNU Affero General Public License for more details.
  -
  - You should have received a copy of the GNU Affero General Public License
  - along with this program. If not, see <http://www.gnu.org/licenses/>.
  -
  -->

<template>
	<DashboardWidget
		id="user-status_panel"
		:items="statuses"
		:loading="loading">
		<template v-slot:default="{ item }">
			<DashboardWidgetItem :item="getWidgetItem(item)">
				{{ item }}
			</DashboardWidgetItem>
		</template>
		<template v-slot:empty-content>
			<EmptyContent icon="icon-user-status">
				{{ t('user_status', 'No one recently updated their status.') }}
			</EmptyContent>
		</template>
	</DashboardWidget>
</template>

<script>
import { DashboardWidget, DashboardWidgetItem } from '@nextcloud/vue-dashboard'
import EmptyContent from '@nextcloud/vue/dist/Components/EmptyContent'
import { loadState } from '@nextcloud/initial-state'

export default {
	name: 'Dashboard',
	components: {
		DashboardWidget,
		DashboardWidgetItem,
		EmptyContent,
	},
	data() {
		return {
			statuses: [],
			loading: true,
		}
	},
	computed: {
		getWidgetItem() {
			return (item) => ({
				mainText: item.displayName,
				subText: `${item.icon || ''} ${item.message || ''}`,
				avatarUsername: item.userId,
			})
		},
	},
	mounted() {
		try {
			this.statuses = loadState('user_status', 'dashboard_data')
			this.loading = false
		} catch (e) {
			console.error(e)
		}
	},
}
</script>

<style>

</style>
